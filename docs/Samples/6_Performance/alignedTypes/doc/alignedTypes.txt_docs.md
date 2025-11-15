# Documentation: Samples/6_Performance/alignedTypes/doc/alignedTypes.txt
---
## File Metadata
- **Path**: `Samples/6_Performance/alignedTypes/doc/alignedTypes.txt`
- **Filename**: `alignedTypes.txt`
- **Language**: text
- **Size**: 761 bytes
- **Lines**: 11
- **Generated**: 2025-11-15 12:53:53 UTC

---
## Original Source
```text
CUDA programming language, being a C with extensions, offers the ability to use arbitrary data structures in GPU programs. But in order for the hardware to perform efficient global loads and stores with variables of structured types, additional alignment details must be specified.

Take a look at this structure definition:
typedef struct{
    float a;
    float b;
} testStructure;

Without alignment specification the compiler will not automatically use a single 64-bit global memory load/store instruction, but will emit two 32-bit load instructions instead.
This significantly impacts aggregate load/store bandwidth, since the latter breaks coalescing rules because of incontiguous memory access pattern. Refer to section 5.1.2.1 of the Programming Guide.

```

---
## High-Level Overview
This file is a text source file in the CUDA Samples repository.


---
## Detailed Walkthrough

---
## Usage Examples
Refer to the repository documentation for usage instructions.


---
## Performance & Security Notes
### Security Considerations
- Review buffer sizes and array bounds
- Validate input parameters
- Check for resource leaks (memory, file handles)


---
## Related Files
(Links to related files will be populated during the folder analysis phase)


---
## Testing & Validation
Refer to the repository's test suite and build instructions.

To build CUDA samples:
```bash
make
```

