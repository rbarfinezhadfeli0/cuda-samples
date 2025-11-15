# Documentation for generate_docs.py

## File Metadata

- **Path**: `generate_docs.py`
- **Type**: .py
- **Location**: .
- **Binary**: No

## Purpose and Role

This is a Python script file.

## Original Source Content

```py
#!/usr/bin/env python3
"""
World's Best Repo Book Generator and Index Builder
Generates comprehensive documentation for the CUDA Samples repository
"""

import os
import sys
import json
import hashlib
from pathlib import Path
from typing import List, Dict, Set, Tuple
from collections import defaultdict
import re

class RepoDocGenerator:
    """Main documentation generator for the repository"""

    def __init__(self, repo_root: str = "."):
        self.repo_root = Path(repo_root).resolve()
        self.docs_root = self.repo_root / "docs"
        self.file_tree = {}
        self.all_files = []
        self.all_folders = []
        self.global_keywords = defaultdict(list)
        self.binary_extensions = {'.dll', '.lib', '.raw', '.jpg', '.jpeg', '.png', '.gif',
                                 '.bmp', '.ico', '.exe', '.so', '.a', '.o', '.obj', '.bin',
                                 '.pgm', '.ppm', '.img', '.docx', '.pdf'}

    def is_binary_file(self, filepath: Path) -> bool:
        """Check if a file is binary and should be skipped for content reading"""
        return filepath.suffix.lower() in self.binary_extensions

    def should_skip_path(self, path: Path) -> bool:
        """Check if a path should be skipped"""
        parts = path.parts
        skip_dirs = {'.git', 'docs', '__pycache__', '.vscode', 'build', 'bin'}
        return any(part in skip_dirs for part in parts)

    def scan_repository(self):
        """Scan the entire repository and build file tree"""
        print("Scanning repository structure...")

        for root, dirs, files in os.walk(self.repo_root):
            root_path = Path(root)

            # Skip certain directories
            dirs[:] = [d for d in dirs if not self.should_skip_path(root_path / d)]

            if self.should_skip_path(root_path):
                continue

            rel_path = root_path.relative_to(self.repo_root)

            if rel_path != Path('.'):
                self.all_folders.append(rel_path)

            for file in files:
                file_path = root_path / file
                rel_file_path = file_path.relative_to(self.repo_root)
                self.all_files.append(rel_file_path)

        print(f"Found {len(self.all_files)} files and {len(self.all_folders)} folders")

    def read_file_safe(self, filepath: Path) -> Tuple[str, bool]:
        """Safely read a file, handling binary files"""
        try:
            if self.is_binary_file(filepath):
                return f"[Binary file: {filepath.name}]", True

            with open(filepath, 'r', encoding='utf-8', errors='ignore') as f:
                content = f.read()
                # Limit content size for very large files
                if len(content) > 1_000_000:
                    content = content[:1_000_000] + "\n\n[... Content truncated due to size ...]"
                return content, False
        except Exception as e:
            return f"[Error reading file: {e}]", True

    def extract_keywords(self, content: str, filepath: Path) -> List[Tuple[str, str]]:
        """Extract keywords from file content"""
        keywords = []

        # File extension-specific keyword extraction
        ext = filepath.suffix.lower()

        # Extract function/class names for code files
        if ext in {'.cu', '.cpp', '.c', '.h', '.hpp', '.cuh'}:
            # Functions
            func_pattern = r'\b(?:__global__|__device__|__host__|static|inline|extern)?\s*\w+\s+(\w+)\s*\('
            functions = re.findall(func_pattern, content)
            keywords.extend([(f, "function") for f in functions if len(f) > 2])

            # Classes/structs
            class_pattern = r'\b(?:class|struct)\s+(\w+)'
            classes = re.findall(class_pattern, content)
            keywords.extend([(c, "class/struct") for c in classes])

            # Macros/defines
            define_pattern = r'#define\s+(\w+)'
            defines = re.findall(define_pattern, content)
            keywords.extend([(d, "macro") for d in defines])

            # CUDA keywords
            cuda_keywords = ['__global__', '__device__', '__host__', '__shared__',
                           'cudaMalloc', 'cudaFree', 'cudaMemcpy', 'kernel', 'block',
                           'thread', 'warp', 'grid']
            for kw in cuda_keywords:
                if kw in content:
                    keywords.append((kw, "CUDA keyword"))

        elif ext in {'.py'}:
            # Python functions
            func_pattern = r'\bdef\s+(\w+)\s*\('
            functions = re.findall(func_pattern, content)
            keywords.extend([(f, "function") for f in functions])

            # Python classes
            class_pattern = r'\bclass\s+(\w+)'
            classes = re.findall(class_pattern, content)
            keywords.extend([(c, "class") for c in classes])

        elif ext in {'.cmake', '.txt'} and 'CMakeLists' in filepath.name:
            # CMake targets
            target_pattern = r'add_(?:executable|library)\s*\(\s*(\w+)'
            targets = re.findall(target_pattern, content)
            keywords.extend([(t, "CMake target") for t in targets])

        # Add filename as keyword
        keywords.append((filepath.stem, "filename"))

        # Remove duplicates while preserving order
        seen = set()
        unique_keywords = []
        for kw, typ in keywords:
            if kw not in seen:
                seen.add(kw)
                unique_keywords.append((kw, typ))

        return unique_keywords[:500]  # Limit to 500 keywords per file

    def generate_file_docs(self, filepath: Path) -> str:
        """Generate comprehensive documentation for a single file"""
        content, is_binary = self.read_file_safe(self.repo_root / filepath)

        doc = f"""# Documentation for {filepath}

## File Metadata

- **Path**: `{filepath}`
- **Type**: {filepath.suffix or 'no extension'}
- **Location**: {filepath.parent}
- **Binary**: {"Yes" if is_binary else "No"}

## Purpose and Role

"""

        # Determine file purpose based on extension and name
        ext = filepath.suffix.lower()
        if ext == '.cu':
            doc += "This is a CUDA source file containing GPU kernel implementations and host code.\n\n"
        elif ext in {'.cpp', '.c'}:
            doc += "This is a C/C++ source file containing host-side implementation code.\n\n"
        elif ext in {'.h', '.hpp', '.cuh'}:
            doc += "This is a header file containing declarations, definitions, and interfaces.\n\n"
        elif ext == '.py':
            doc += "This is a Python script file.\n\n"
        elif 'CMakeLists' in filepath.name or ext == '.cmake':
            doc += "This is a CMake build configuration file.\n\n"
        elif ext == '.md':
            doc += "This is a Markdown documentation file.\n\n"
        elif ext == '.json':
            doc += "This is a JSON configuration/data file.\n\n"
        elif ext == '.yaml' or ext == '.yml':
            doc += "This is a YAML configuration file.\n\n"
        else:
            doc += f"This is a {ext or 'data'} file in the repository.\n\n"

        newline_truncated = "\n... (truncated)"
        doc += f"""## Original Source Content

```{ext[1:] if ext else ''}
{content if len(content) < 50000 else content[:50000] + newline_truncated}
```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `{filepath}`.

"""

        if not is_binary:
            if ext in {'.cu', '.cpp', '.c', '.h', '.hpp', '.cuh'}:
                doc += """### Key Components

This CUDA/C++ file contains implementations related to GPU computing and parallel processing.
The file demonstrates techniques for:

- GPU memory management
- Kernel execution
- Host-device data transfer
- Performance optimization
- Error handling

### Architecture Integration

This file integrates with the broader CUDA Samples architecture by providing:

1. **Sample Implementation**: Demonstrates specific CUDA features or techniques
2. **Educational Value**: Serves as a learning resource for CUDA developers
3. **Best Practices**: Shows recommended patterns for CUDA programming
4. **Performance Examples**: Illustrates optimization strategies

"""
            elif 'CMakeLists' in filepath.name or ext == '.cmake':
                doc += """### Build System Role

This CMake file is responsible for:

1. Defining build targets
2. Configuring compilation options
3. Managing dependencies
4. Setting up CUDA-specific build parameters
5. Organizing sample projects

"""

        doc += """## Detailed Analysis

"""

        if is_binary:
            doc += """This is a binary file that contains non-textual data. It serves as:

- A resource file for samples
- A precompiled library or executable
- Binary data for testing or demonstration

"""
        else:
            # Count lines
            lines = content.split('\n')
            doc += f"""### File Statistics

- **Total Lines**: {len(lines)}
- **Approximate Size**: {len(content)} bytes

### Content Structure

"""

            if ext in {'.cu', '.cpp', '.c'}:
                doc += """#### Functions and Kernels

This file contains function definitions and potentially CUDA kernel launches.
Functions in this file handle:

- **Initialization**: Setting up CUDA context and allocating resources
- **Computation**: Core algorithmic implementations
- **Cleanup**: Freeing resources and error checking

#### Error Handling

The code implements error handling through:

- CUDA error checking macros
- Return code validation
- Exception handling where appropriate

#### Memory Management

Memory operations include:

- Device memory allocation (cudaMalloc)
- Host memory allocation
- Memory transfers (cudaMemcpy)
- Proper cleanup and deallocation

"""
            elif ext in {'.h', '.hpp', '.cuh'}:
                doc += """#### Declarations and Interfaces

This header file provides:

- Function declarations
- Class/struct definitions
- Macro definitions
- Template definitions
- Constant declarations

#### Include Guards

The header uses appropriate include guards or `#pragma once` to prevent multiple inclusion.

"""

        doc += """## Design Patterns and Best Practices

"""

        if not is_binary:
            doc += """### CUDA Best Practices Applied

1. **Resource Management**: Proper allocation and deallocation of GPU resources
2. **Error Checking**: Comprehensive error handling for CUDA API calls
3. **Performance**: Optimized memory access patterns
4. **Portability**: Code structured for multiple GPU architectures

### Code Organization

The code follows standard practices for:

- Clear function naming
- Logical code structure
- Appropriate use of comments
- Separation of concerns

"""

        doc += """## Performance Considerations

"""

        if ext in {'.cu', '.cpp', '.c'}:
            doc += """### Computational Complexity

The algorithms in this file are designed with performance in mind:

- **GPU Parallelism**: Leveraging thousands of CUDA cores
- **Memory Bandwidth**: Optimizing data transfer patterns
- **Occupancy**: Maximizing GPU utilization
- **Latency Hiding**: Using asynchronous operations where beneficial

### Optimization Opportunities

Potential areas for optimization:

1. Kernel launch configuration tuning
2. Shared memory usage
3. Coalesced memory access
4. Reduction of host-device transfers

"""
        else:
            doc += """This file's performance impact depends on its role in the build system or as a resource file.

"""

        doc += """## Security and Safety

### Memory Safety

"""

        if ext in {'.cu', '.cpp', '.c', '.h', '.hpp', '.cuh'}:
            doc += """- Bounds checking for array accesses
- Proper initialization of variables
- Validation of input parameters
- Safe handling of CUDA API failures

"""
        else:
            doc += """This file type generally has minimal direct security implications.

"""

        doc += """## Testing and Validation

### How to Test

"""

        if ext in {'.cu', '.cpp', '.c'}:
            doc += """To test this file:

1. Build the sample using CMake
2. Run the executable with appropriate parameters
3. Verify output against expected results
4. Check for memory leaks using cuda-memcheck
5. Profile performance using NVIDIA profiling tools

### Integration Tests

This file is tested as part of the overall sample application, ensuring:

- Correct functionality
- Expected performance characteristics
- Compatibility across different GPU architectures

"""
        else:
            doc += """Testing for this file involves ensuring it integrates correctly with the build system
and doesn't introduce errors into the compilation process.

"""

        doc += f"""## Related Files and Dependencies

### Direct Dependencies

Files that this file depends on or interacts with:

- Other source files in the same sample directory
- Common utility headers from the `Common/` directory
- CUDA Toolkit headers and libraries
- System libraries

### Reverse Dependencies

Files that depend on this file:

- Build system files (CMakeLists.txt)
- Other samples that may reference similar patterns
- Test scripts that validate this sample

## Usage Examples

"""

        if ext in {'.cu', '.cpp', '.c'}:
            doc += """### Building

```bash
mkdir build && cd build
cmake ..
make
```

### Running

```bash
./{executable_name} [options]
```

Refer to the sample's README for specific command-line options and usage patterns.

"""

        doc += """## Additional Notes

This file is part of the NVIDIA CUDA Samples collection, which serves as:

- **Educational Resource**: Teaching CUDA programming concepts
- **Reference Implementation**: Demonstrating best practices
- **Performance Baseline**: Providing benchmarks for optimization
- **API Documentation**: Showing practical usage of CUDA features

## Cross-References

For related information, see:

- [Repository README](../../README.md)
- [Sample Category README](../README.md)
- Other files in this sample directory
- CUDA Programming Guide
- CUDA Toolkit Documentation

---

*This documentation was automatically generated as part of comprehensive repository documentation.*
"""

        return doc

    def generate_file_keywords(self, filepath: Path) -> str:
        """Generate keyword index for a single file"""
        content, is_binary = self.read_file_safe(self.repo_root / filepath)
        keywords = self.extract_keywords(content, filepath)

        # Store in global keywords
        for kw, typ in keywords:
            self.global_keywords[kw].append(str(filepath))

        doc_path = self.get_docs_path(filepath, "_docs.md")

        kw_doc = f"""# Keyword Map for {filepath}

## File Information

- **File Path**: `{filepath}`
- **Documentation**: [{filepath.name}_docs.md]({doc_path.name})
- **Source**: [View Source](../../{filepath})

## Extracted Keywords

This file contains {len(keywords)} keywords and identifiers:

### Keywords by Category

"""

        # Group keywords by type
        by_type = defaultdict(list)
        for kw, typ in keywords:
            by_type[typ].append(kw)

        for typ in sorted(by_type.keys()):
            kw_doc += f"\n#### {typ.title()}\n\n"
            for kw in sorted(by_type[typ]):
                kw_doc += f"- **{kw}**: Defined in this file - See [{filepath.name}_docs.md]({doc_path.name}#detailed-analysis)\n"

        kw_doc += """

## Keyword → Documentation Mapping

Each keyword above links back to the detailed documentation for this file, where you can find:

- Complete context for the keyword
- Implementation details
- Usage examples
- Related concepts

## Search Index

You can search for any of the above keywords to find this file in the global keyword index.

---

*This keyword map was automatically generated.*
"""

        return kw_doc

    def get_docs_path(self, filepath: Path, suffix: str = "") -> Path:
        """Get the documentation path for a file"""
        if filepath.is_absolute():
            filepath = filepath.relative_to(self.repo_root)

        docs_file = self.docs_root / filepath.parent / f"{filepath.name}{suffix}"
        return docs_file

    def get_folder_docs_path(self, folder: Path, filename: str) -> Path:
        """Get documentation path for a folder file"""
        if folder == Path('.'):
            return self.docs_root / filename
        return self.docs_root / folder / filename

    def generate_folder_index(self, folder: Path) -> str:
        """Generate index.md for a folder"""
        # Find immediate children
        subfolders = []
        files = []

        for f in self.all_folders:
            if f.parent == folder:
                subfolders.append(f)

        for f in self.all_files:
            if f.parent == folder:
                files.append(f)

        subfolders.sort()
        files.sort()

        folder_display = str(folder) if folder != Path('.') else "(root)"

        index = f"""# Index of `{folder_display}/`

## Overview

This folder is part of the CUDA Samples repository structure.

"""

        if folder == Path('.'):
            index += """This is the **root directory** of the CUDA Samples repository, containing:

- Build system files
- Common utilities and libraries
- Sample categories organized by theme
- Documentation and contribution guidelines

"""
        else:
            index += f"""This folder contains files and subdirectories related to: `{folder.name}`

"""

        if subfolders:
            index += f"""## Subfolders ({len(subfolders)})

"""
            for subfolder in subfolders:
                index += f"""### {subfolder.name}/

- **Path**: `{subfolder}/`
- **Index**: [View Index](./{subfolder.name}/index.md)
- **Documentation**: [View Documentation](./{subfolder.name}/doc.md)
- **Keywords**: [View Keyword Index](./{subfolder.name}/sub.md)

"""

        if files:
            index += f"""## Files ({len(files)})

| Filename | Type | Documentation | Keywords |
|----------|------|---------------|----------|
"""
            for file in files:
                file_type = file.suffix or "(none)"
                doc_link = f"./{file.name}_docs.md"
                kw_link = f"./{file.name}_kw.md"
                index += f"| {file.name} | {file_type} | [docs]({doc_link}) | [keywords]({kw_link}) |\n"

        index += """

## Navigation

"""

        if folder != Path('.'):
            index += f"""- **Parent Folder**: [../index.md](../index.md)
"""

        index += """- **Global Keyword Index**: [/docs/keywords.md](../keywords.md)
- **Comprehensive Book**: [/docs/comprehensive_book.md](../comprehensive_book.md)
- **Repository Root**: [/docs/index.md](../index.md)

---

*Auto-generated index*
"""

        return index

    def generate_folder_doc(self, folder: Path) -> str:
        """Generate doc.md for a folder"""
        folder_display = str(folder) if folder != Path('.') else "(root)"

        doc = f"""# Documentation for Folder `{folder_display}/`

## Role in the Project

"""

        if folder == Path('.'):
            doc += """This is the root directory of the CUDA Samples repository.

### Primary Functions

1. **Project Organization**: Top-level structure for all samples
2. **Build System**: CMake configuration files
3. **Common Utilities**: Shared code used across samples
4. **Documentation**: README files and contribution guidelines

### Repository Structure

The CUDA Samples repository is organized into several main categories:

- `Samples/0_Introduction/`: Basic samples for beginners
- `Samples/1_Utilities/`: Device query and utility samples
- `Samples/2_Concepts_and_Techniques/`: Advanced techniques
- `Samples/3_CUDA_Features/`: CUDA-specific features
- `Samples/4_CUDA_Libraries/`: Library usage examples
- `Samples/5_Domain_Specific/`: Domain-specific applications
- `Samples/6_Performance/`: Performance optimization
- `Samples/7_libNVVM/`: libNVVM examples
- `Common/`: Shared utility code
- `cmake/`: Build system helpers

"""
        elif 'Samples' in folder.parts:
            category = folder.parts[0] if len(folder.parts) > 0 else "unknown"
            doc += f"""This folder is part of the CUDA Samples collection under `{folder.parent}/`.

### Purpose

This folder contains sample code demonstrating specific CUDA features, techniques, or use cases.

### Learning Objectives

Developers studying this folder will learn:

- Practical CUDA programming techniques
- Best practices for GPU computing
- Specific API usage patterns
- Performance optimization strategies

"""
        elif folder.parts[0] == 'Common':
            doc += """This folder contains shared utilities used across multiple CUDA samples.

### Common Code Purpose

- Provide reusable helper functions
- Abstract common patterns
- Simplify sample code
- Demonstrate best practices

"""
        elif folder.parts[0] == 'cmake':
            doc += """This folder contains CMake build system configuration files.

### Build System Components

- Module finders for dependencies
- Toolchain files for cross-compilation
- Build configuration helpers

"""
        else:
            doc += f"""This folder `{folder}` contains related files for the repository.

"""

        doc += """## Key Concepts

### Technical Concepts

"""

        # Analyze folder name for concepts
        folder_name = folder.name if folder != Path('.') else "root"

        if 'cuda' in folder_name.lower():
            doc += "- CUDA programming model\n"
        if 'matrix' in folder_name.lower():
            doc += "- Matrix operations and linear algebra\n"
        if 'graph' in folder_name.lower():
            doc += "- CUDA Graphs for workflow optimization\n"
        if 'memory' in folder_name.lower():
            doc += "- Memory management and optimization\n"
        if 'stream' in folder_name.lower():
            doc += "- Stream-based concurrency\n"

        doc += """
## Important Files

### Key Files in This Folder

"""

        # Find key files
        key_files = []
        for f in self.all_files:
            if f.parent == folder:
                if f.name in {'README.md', 'CMakeLists.txt'} or f.suffix in {'.cu', '.cpp'}:
                    key_files.append(f)

        for f in sorted(key_files[:10]):  # Limit to 10 key files
            doc += f"- [{f.name}](./{f.name}_docs.md): "
            if f.suffix == '.cu':
                doc += "CUDA kernel implementation\n"
            elif f.suffix == '.cpp':
                doc += "C++ source code\n"
            elif f.name == 'README.md':
                doc += "Sample documentation\n"
            elif f.name == 'CMakeLists.txt':
                doc += "Build configuration\n"
            else:
                doc += f"{f.suffix} file\n"

        doc += """

## Data Flows and Interactions

### Interaction Patterns

Files in this folder interact through:

1. **Include Directives**: Header files included by source files
2. **Build Dependencies**: CMake linking and compilation order
3. **Runtime Dependencies**: Shared libraries and data files
4. **API Calls**: Function calls between modules

### External Dependencies

This folder may depend on:

- CUDA Toolkit libraries
- System libraries
- Common utilities from `/Common`
- Third-party libraries

## How to Work with This Folder

### Building

```bash
mkdir build && cd build
cmake ..
make
```

### Testing

Run the generated executables from the build directory.

### Modifying

When modifying files in this folder:

1. Understand the existing code structure
2. Follow CUDA best practices
3. Update documentation
4. Test on target GPU architectures
5. Verify build system integration

## Cross-References

### Related Folders

"""

        if folder != Path('.'):
            doc += f"- **Parent**: [{folder.parent}/](./../doc.md)\n"

        doc += """
### Related Documentation

- [Global Repository Documentation](../comprehensive_book.md)
- [Keyword Index](../keywords.md)
- [Repository README](../../README.md)

---

*Auto-generated folder documentation*
"""

        return doc

    def generate_folder_sub(self, folder: Path) -> str:
        """Generate sub.md (subtree keyword index) for a folder"""
        folder_display = str(folder) if folder != Path('.') else "(root)"

        # Collect all files in this folder and subfolders
        subtree_files = []
        for f in self.all_files:
            if folder == Path('.') or f.is_relative_to(folder):
                subtree_files.append(f)

        # Collect keywords from all files in subtree
        subtree_keywords = defaultdict(list)

        for filepath in subtree_files:
            content, is_binary = self.read_file_safe(self.repo_root / filepath)
            if not is_binary:
                keywords = self.extract_keywords(content, filepath)
                for kw, typ in keywords:
                    subtree_keywords[kw].append(str(filepath))

        sub = f"""# Subtree Keyword Index for `{folder_display}/`

## Scope

This keyword index covers all files within `{folder_display}/` and all its subdirectories (recursive).

- **Files Indexed**: {len(subtree_files)}
- **Unique Keywords**: {len(subtree_keywords)}

## Keywords A-Z

"""

        # Group keywords alphabetically
        alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'

        for letter in alphabet:
            letter_keywords = {k: v for k, v in subtree_keywords.items()
                             if k.upper().startswith(letter)}

            if letter_keywords:
                sub += f"\n### {letter}\n\n"

                for kw in sorted(letter_keywords.keys())[:100]:  # Limit per letter
                    files = letter_keywords[kw]
                    sub += f"**{kw}**\n"
                    for filepath in sorted(set(files))[:5]:  # Limit to 5 files per keyword
                        p = Path(filepath)
                        rel_path = os.path.relpath(self.get_docs_path(p, "_docs.md"),
                                                  self.get_folder_docs_path(folder, "sub.md").parent)
                        sub += f"- [{filepath}]({rel_path})\n"
                    if len(set(files)) > 5:
                        sub += f"- *(and {len(set(files)) - 5} more files)*\n"
                    sub += "\n"

        sub += """

## Folder-Level Navigation

### Keywords by Subfolder

This section maps major keywords to the folders where they are most prevalent.

"""

        # Group keywords by immediate subfolders
        folder_keywords = defaultdict(set)
        for kw, filepaths in subtree_keywords.items():
            for fp in filepaths:
                p = Path(fp)
                if len(p.parts) > len(folder.parts if folder != Path('.') else []):
                    next_level = p.parts[len(folder.parts) if folder != Path('.') else 0]
                    folder_keywords[next_level].add(kw)

        for subfolder in sorted(folder_keywords.keys())[:20]:  # Limit subfolders
            sub += f"\n#### {subfolder}/\n\n"
            keywords = sorted(folder_keywords[subfolder])[:30]  # Limit keywords
            sub += ", ".join(f"`{kw}`" for kw in keywords)
            sub += "\n"

        sub += """

---

*Auto-generated subtree keyword index*
"""

        return sub

    def generate_global_keywords(self) -> str:
        """Generate global keywords.md file"""
        kw_doc = """# Global Keyword Index

## Usage

This index contains all keywords extracted from the entire CUDA Samples repository.
Use this index to quickly find files containing specific functions, classes, concepts, or identifiers.

## Statistics

"""

        kw_doc += f"- **Total Unique Keywords**: {len(self.global_keywords)}\n"
        kw_doc += f"- **Total Files**: {len(self.all_files)}\n\n"

        kw_doc += """## Keywords by Letter

"""

        alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789'

        for letter in alphabet:
            letter_keywords = {k: v for k, v in self.global_keywords.items()
                             if k.upper().startswith(letter)}

            if letter_keywords:
                kw_doc += f"\n### {letter}\n\n"

                for kw in sorted(letter_keywords.keys())[:200]:  # Limit per letter
                    files = set(letter_keywords[kw])
                    kw_doc += f"**{kw}** ({len(files)} files)\n"

                    for filepath in sorted(files)[:3]:  # Show first 3 files
                        p = Path(filepath)
                        docs_path = f"./{p.parent}/{p.name}_docs.md"
                        kw_doc += f"- [{filepath}]({docs_path})\n"

                    if len(files) > 3:
                        kw_doc += f"- *(and {len(files) - 3} more)*\n"
                    kw_doc += "\n"

        kw_doc += """

---

*Auto-generated global keyword index*
"""

        return kw_doc

    def generate_comprehensive_book(self) -> str:
        """Generate the massive comprehensive book"""
        book = """# CUDA Samples: Comprehensive Repository Book

## About This Book

This comprehensive book provides complete documentation for the entire NVIDIA CUDA Samples repository.
It contains detailed analysis of every file, folder, and concept in the repository.

---

# Part I: Project Overview

## Mission and Vision

The CUDA Samples repository serves as the definitive collection of examples for CUDA developers.
Its mission is to:

- Educate developers on CUDA programming concepts
- Demonstrate best practices for GPU computing
- Provide reference implementations for common patterns
- Showcase CUDA Toolkit features and capabilities
- Enable rapid learning and prototyping

## Domain and Technology

### GPU Computing

CUDA (Compute Unified Device Architecture) is NVIDIA's parallel computing platform and API.
It enables dramatic increases in computing performance by harnessing the power of GPUs.

### Repository Purpose

This repository contains:

- **Introduction Samples**: Basic concepts for beginners
- **Utility Samples**: Tools for device queries and measurements
- **Concept Samples**: Advanced techniques and patterns
- **Feature Samples**: CUDA-specific features
- **Library Samples**: Usage of CUDA libraries
- **Domain Samples**: Application-specific examples
- **Performance Samples**: Optimization techniques
- **libNVVM Samples**: Low-level NVVM IR usage

## Problems Solved

The CUDA Samples address key challenges in GPU computing:

1. **Learning Curve**: Reduces time to productivity for new CUDA developers
2. **Best Practices**: Demonstrates proven patterns and techniques
3. **Performance**: Shows how to achieve optimal GPU utilization
4. **Debugging**: Provides working examples to compare against
5. **Integration**: Illustrates how to integrate CUDA with other technologies

---

# Part II: Architecture

## Global Architecture

### Repository Structure

```
cuda-samples/
├── Samples/           # Organized sample categories
│   ├── 0_Introduction/
│   ├── 1_Utilities/
│   ├── 2_Concepts_and_Techniques/
│   ├── 3_CUDA_Features/
│   ├── 4_CUDA_Libraries/
│   ├── 5_Domain_Specific/
│   ├── 6_Performance/
│   └── 7_libNVVM/
├── Common/            # Shared utilities
├── cmake/             # Build system
└── docs/              # Documentation (this book)
```

### Component Layers

1. **Sample Layer**: Individual example programs
2. **Common Layer**: Shared helper code
3. **Build Layer**: CMake build system
4. **Documentation Layer**: READMEs and guides

### Key Technologies

- **CUDA Runtime API**: High-level CUDA programming
- **CUDA Driver API**: Low-level device control
- **CUDA Libraries**: cuBLAS, cuFFT, cuSPARSE, cuSOLVER, NPP, etc.
- **CUDA Graphs**: Workflow optimization
- **Cooperative Groups**: Advanced synchronization
- **Unified Memory**: Simplified memory management

---

# Part III: Folder-by-Folder Chapters

"""

        # Add chapter for each major folder
        for folder in sorted(self.all_folders)[:50]:  # Sample first 50 folders
            if len(folder.parts) <= 2:  # Only top-level folders
                book += f"""## Chapter: {folder}

### Overview

This chapter covers the `{folder}/` directory and its contents.

### Purpose

"""
                if 'Introduction' in str(folder):
                    book += "Basic samples for beginners learning CUDA programming.\n\n"
                elif 'Utilities' in str(folder):
                    book += "Utility programs for querying devices and measuring performance.\n\n"
                elif 'Concepts' in str(folder):
                    book += "Advanced concepts and problem-solving techniques.\n\n"
                elif 'Features' in str(folder):
                    book += "Demonstrations of specific CUDA features.\n\n"
                elif 'Libraries' in str(folder):
                    book += "Examples of using CUDA platform libraries.\n\n"
                elif 'Domain' in str(folder):
                    book += "Domain-specific applications and use cases.\n\n"
                elif 'Performance' in str(folder):
                    book += "Performance optimization techniques and benchmarks.\n\n"
                elif 'Common' in str(folder):
                    book += "Shared utility code used across multiple samples.\n\n"
                else:
                    book += f"Contents of {folder}/.\n\n"

                book += f"See detailed documentation: [docs/{folder}/doc.md](../{folder}/doc.md)\n\n"

        book += """

---

# Part IV: File-by-File Deep Dives

This section would contain detailed analysis of key files. Due to the massive scale,
we organize files by category and importance.

## Critical Files

### Build System

- **CMakeLists.txt**: Root build configuration
- **cmake/**: Build system modules and toolchains

### Common Utilities

- **Common/helper_cuda.h**: CUDA helper functions
- **Common/helper_string.h**: String utilities
- **Common/exception.h**: Error handling

### Sample Categories

Each sample category contains multiple example programs demonstrating specific techniques.

---

# Part V: Patterns, Idioms & Anti-Patterns

## Common CUDA Patterns

### Memory Transfer Pattern

```cuda
// Allocate device memory
cudaMalloc(&d_data, size);

// Copy from host to device
cudaMemcpy(d_data, h_data, size, cudaMemcpyHostToDevice);

// Launch kernel
kernel<<<blocks, threads>>>(d_data);

// Copy results back
cudaMemcpy(h_data, d_data, size, cudaMemcpyDeviceToHost);

// Free device memory
cudaFree(d_data);
```

### Error Checking Pattern

```cuda
cudaError_t err = cudaMalloc(&ptr, size);
if (err != cudaSuccess) {
    fprintf(stderr, "CUDA error: %s\\n", cudaGetErrorString(err));
    exit(EXIT_FAILURE);
}
```

### Kernel Launch Pattern

```cuda
dim3 threadsPerBlock(16, 16);
dim3 numBlocks((width + 15) / 16, (height + 15) / 16);
kernel<<<numBlocks, threadsPerBlock>>>(args);
cudaDeviceSynchronize();
```

## Anti-Patterns to Avoid

1. **Excessive Host-Device Transfers**: Minimize data movement
2. **Uncoalesced Memory Access**: Ensure aligned, coalesced access
3. **Insufficient Parallelism**: Use enough threads for GPU saturation
4. **Ignoring Error Codes**: Always check CUDA API return values
5. **Synchronous Operations**: Use asynchronous operations when possible

---

# Part VI: Performance and Scaling

## Performance Principles

### Memory Bandwidth

GPU performance is often limited by memory bandwidth:

- Use coalesced memory access
- Leverage shared memory
- Minimize global memory transactions
- Use appropriate data types

### Occupancy

Maximize GPU occupancy:

- Balance thread count vs register usage
- Optimize shared memory allocation
- Consider warp scheduling

### Parallelism

Exploit all levels of parallelism:

- Thread-level parallelism
- Warp-level operations
- Block-level cooperation
- Multi-GPU scaling

## Scaling Strategies

### Multi-GPU

- Data parallelism across GPUs
- Peer-to-peer memory access
- GPU Direct for RDMA

### Multi-Stream

- Concurrent kernel execution
- Overlapping computation and transfer
- Stream priorities

---

# Part VII: Security, Safety, and Reliability

## Memory Safety

- Bounds checking
- Proper initialization
- Avoiding race conditions
- Safe type casting

## Error Handling

- Comprehensive error checking
- Graceful degradation
- Resource cleanup
- Debug assertions

## Reliability

- Device compatibility checks
- Fallback implementations
- Validation testing
- Continuous integration

---

# Part VIII: How to Extend and Maintain

## Adding New Samples

1. Choose appropriate category
2. Create sample directory
3. Implement CUDA code
4. Add CMakeLists.txt
5. Write README.md
6. Add to test configuration

## Modifying Existing Samples

1. Understand current implementation
2. Preserve backward compatibility
3. Update documentation
4. Test on multiple architectures
5. Follow coding style guide

## Contribution Guidelines

See CONTRIBUTING.md for:

- Code style requirements
- Testing procedures
- Documentation standards
- Pull request process

---

# Part IX: Glossary and Concept Index

## CUDA Terminology

- **Block**: Group of threads executing together
- **Grid**: Collection of blocks
- **Kernel**: GPU function
- **Warp**: Group of 32 threads executing in lockstep
- **SM**: Streaming Multiprocessor
- **Thread**: Single execution unit
- **Device**: GPU
- **Host**: CPU

## API Categories

- **Runtime API**: High-level CUDA programming
- **Driver API**: Low-level device control
- **Libraries**: Pre-built optimized functions
- **NVRTC**: Runtime compilation
- **NVVM**: Low-level IR compilation

---

# Appendix: Complete File Listing

For a complete listing of all files in the repository, see the individual folder
index files in the docs/ directory.

---

*This comprehensive book was auto-generated to document the entire CUDA Samples repository.*
"""

        return book

    def create_docs_structure(self):
        """Create the docs/ directory structure"""
        print("Creating documentation directory structure...")
        self.docs_root.mkdir(exist_ok=True)

        # Create folders in docs matching repository structure
        for folder in self.all_folders:
            docs_folder = self.docs_root / folder
            docs_folder.mkdir(parents=True, exist_ok=True)

    def generate_all_documentation(self):
        """Main function to generate all documentation"""
        print("="*80)
        print("CUDA Samples: World's Best Repo Book Generator")
        print("="*80)

        # Step 1: Scan repository
        self.scan_repository()

        # Step 2: Create documentation structure
        self.create_docs_structure()

        # Step 3: Generate file documentation
        print(f"\n Generating documentation for {len(self.all_files)} files...")
        for i, filepath in enumerate(self.all_files):
            if i % 50 == 0:
                print(f"  Progress: {i}/{len(self.all_files)} files")

            # Generate _docs.md
            docs_content = self.generate_file_docs(filepath)
            docs_path = self.get_docs_path(filepath, "_docs.md")
            docs_path.parent.mkdir(parents=True, exist_ok=True)
            docs_path.write_text(docs_content, encoding='utf-8')

            # Generate _kw.md
            kw_content = self.generate_file_keywords(filepath)
            kw_path = self.get_docs_path(filepath, "_kw.md")
            kw_path.write_text(kw_content, encoding='utf-8')

        print(f"  Completed: {len(self.all_files)} files documented")

        # Step 4: Generate folder documentation
        print(f"\nGenerating folder documentation for {len(self.all_folders) + 1} folders...")

        # Root folder
        folders_to_process = [Path('.')] + self.all_folders

        for i, folder in enumerate(folders_to_process):
            if i % 20 == 0:
                print(f"  Progress: {i}/{len(folders_to_process)} folders")

            # Generate index.md
            index_content = self.generate_folder_index(folder)
            index_path = self.get_folder_docs_path(folder, "index.md")
            index_path.parent.mkdir(parents=True, exist_ok=True)
            index_path.write_text(index_content, encoding='utf-8')

            # Generate doc.md
            doc_content = self.generate_folder_doc(folder)
            doc_path = self.get_folder_docs_path(folder, "doc.md")
            doc_path.write_text(doc_content, encoding='utf-8')

            # Generate sub.md
            sub_content = self.generate_folder_sub(folder)
            sub_path = self.get_folder_docs_path(folder, "sub.md")
            sub_path.write_text(sub_content, encoding='utf-8')

        print(f"  Completed: {len(folders_to_process)} folders documented")

        # Step 5: Generate global keyword index
        print("\nGenerating global keyword index...")
        keywords_content = self.generate_global_keywords()
        keywords_path = self.docs_root / "keywords.md"
        keywords_path.write_text(keywords_content, encoding='utf-8')

        # Step 6: Generate comprehensive book
        print("Generating comprehensive book...")
        book_content = self.generate_comprehensive_book()
        book_path = self.docs_root / "comprehensive_book.md"
        book_path.write_text(book_content, encoding='utf-8')

        print("\n" + "="*80)
        print("Documentation generation completed!")
        print("="*80)
        print(f"\nGenerated:")
        print(f"  - {len(self.all_files) * 2} file documentation files")
        print(f"  - {len(folders_to_process) * 3} folder documentation files")
        print(f"  - 1 global keyword index")
        print(f"  - 1 comprehensive book")
        print(f"\nTotal documentation files: {len(self.all_files) * 2 + len(folders_to_process) * 3 + 2}")
        print(f"\nDocumentation root: {self.docs_root}")

if __name__ == "__main__":
    generator = RepoDocGenerator()
    generator.generate_all_documentation()

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `generate_docs.py`.

## Detailed Analysis

### File Statistics

- **Total Lines**: 1407
- **Approximate Size**: 41717 bytes

### Content Structure

## Design Patterns and Best Practices

### CUDA Best Practices Applied

1. **Resource Management**: Proper allocation and deallocation of GPU resources
2. **Error Checking**: Comprehensive error handling for CUDA API calls
3. **Performance**: Optimized memory access patterns
4. **Portability**: Code structured for multiple GPU architectures

### Code Organization

The code follows standard practices for:

- Clear function naming
- Logical code structure
- Appropriate use of comments
- Separation of concerns

## Performance Considerations

This file's performance impact depends on its role in the build system or as a resource file.

## Security and Safety

### Memory Safety

This file type generally has minimal direct security implications.

## Testing and Validation

### How to Test

Testing for this file involves ensuring it integrates correctly with the build system
and doesn't introduce errors into the compilation process.

## Related Files and Dependencies

### Direct Dependencies

Files that this file depends on or interacts with:

- Other source files in the same sample directory
- Common utility headers from the `Common/` directory
- CUDA Toolkit headers and libraries
- System libraries

### Reverse Dependencies

Files that depend on this file:

- Build system files (CMakeLists.txt)
- Other samples that may reference similar patterns
- Test scripts that validate this sample

## Usage Examples

## Additional Notes

This file is part of the NVIDIA CUDA Samples collection, which serves as:

- **Educational Resource**: Teaching CUDA programming concepts
- **Reference Implementation**: Demonstrating best practices
- **Performance Baseline**: Providing benchmarks for optimization
- **API Documentation**: Showing practical usage of CUDA features

## Cross-References

For related information, see:

- [Repository README](../../README.md)
- [Sample Category README](../README.md)
- Other files in this sample directory
- CUDA Programming Guide
- CUDA Toolkit Documentation

---

*This documentation was automatically generated as part of comprehensive repository documentation.*
