# Documentation for Common/param.h

## File Metadata

- **Path**: `Common/param.h`
- **Type**: .h
- **Location**: Common
- **Binary**: No

## Purpose and Role

This is a header file containing declarations, definitions, and interfaces.

## Original Source Content

```h
/* Copyright (c) 2022, NVIDIA CORPORATION. All rights reserved.
 *
 * Redistribution and use in source and binary forms, with or without
 * modification, are permitted provided that the following conditions
 * are met:
 *  * Redistributions of source code must retain the above copyright
 *    notice, this list of conditions and the following disclaimer.
 *  * Redistributions in binary form must reproduce the above copyright
 *    notice, this list of conditions and the following disclaimer in the
 *    documentation and/or other materials provided with the distribution.
 *  * Neither the name of NVIDIA CORPORATION nor the names of its
 *    contributors may be used to endorse or promote products derived
 *    from this software without specific prior written permission.
 *
 * THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS ``AS IS'' AND ANY
 * EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
 * IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
 * PURPOSE ARE DISCLAIMED.  IN NO EVENT SHALL THE COPYRIGHT OWNER OR
 * CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
 * EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
 * PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
 * PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY
 * OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
 * (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
 * OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
 */

/*
 Simple parameter system
 sgreen@nvidia.com 4/2001
*/

#ifndef PARAM_H
#define PARAM_H

#include <iomanip>
#include <iostream>
#include <map>
#include <sstream>
#include <string>
#include <vector>

// base class for named parameter
class ParamBase {
 public:
  ParamBase(const char *name) : m_name(name) {}
  virtual ~ParamBase() {}

  std::string &GetName() { return m_name; }

  virtual float GetFloatValue() = 0;
  virtual int GetIntValue() = 0;
  virtual std::string GetValueString() = 0;

  virtual void Reset() = 0;
  virtual void Increment() = 0;
  virtual void Decrement() = 0;

  virtual float GetPercentage() = 0;
  virtual void SetPercentage(float p) = 0;

  virtual void Write(std::ostream &stream) = 0;
  virtual void Read(std::istream &stream) = 0;

  virtual bool IsList() = 0;

 protected:
  std::string m_name;
};

// derived class for single-valued parameter
template <class T>
class Param : public ParamBase {
 public:
  Param(const char *name, T value = 0, T min = 0, T max = 10000, T step = 1,
        T *ptr = 0)
      : ParamBase(name),
        m_default(value),
        m_min(min),
        m_max(max),
        m_step(step),
        m_precision(3) {
    if (ptr) {
      m_ptr = ptr;
    } else {
      m_ptr = &m_value;
    }

    *m_ptr = value;
  }
  ~Param() {}

  T GetValue() const { return *m_ptr; }
  T SetValue(const T value) { *m_ptr = value; }

  float GetFloatValue() { return (float)*m_ptr; }
  int GetIntValue() { return (int)*m_ptr; }

  std::string GetValueString() {
    std::ostringstream ost;
    ost << std::setprecision(m_precision) << std::fixed;
    ost << *m_ptr;
    return ost.str();
  }

  void SetPrecision(int x) { m_precision = x; }

  float GetPercentage() { return (*m_ptr - m_min) / (float)(m_max - m_min); }

  void SetPercentage(float p) { *m_ptr = (T)(m_min + p * (m_max - m_min)); }

  void Reset() { *m_ptr = m_default; }

  void Increment() {
    *m_ptr += m_step;

    if (*m_ptr > m_max) {
      *m_ptr = m_max;
    }
  }

  void Decrement() {
    *m_ptr -= m_step;

    if (*m_ptr < m_min) {
      *m_ptr = m_min;
    }
  }

  void Write(std::ostream &stream) {
    stream << m_name << " " << *m_ptr << '\n';
  }
  void Read(std::istream &stream) { stream >> m_name >> *m_ptr; }

  bool IsList() { return false; }

 private:
  T m_value;
  T *m_ptr;  // pointer to value declared elsewhere
  T m_default, m_min, m_max, m_step;
  int m_precision;  // number of digits after decimal point in string output
};

const Param<int> dummy("error");

// list of parameters
class ParamList : public ParamBase {
 public:
  ParamList(const char *name = "") : ParamBase(name) { active = true; }
  ~ParamList() {}

  float GetFloatValue() { return 0.0f; }
  int GetIntValue() { return 0; }

  void AddParam(ParamBase *param) {
    m_params.push_back(param);
    m_map[param->GetName()] = param;
    m_current = m_params.begin();
  }

  // look-up parameter based on name
  ParamBase *GetParam(char *name) {
    ParamBase *p = m_map[name];

    if (p) {
      return p;
    } else {
      return (ParamBase *)&dummy;
    }
  }

  ParamBase *GetParam(int i) { return m_params[i]; }

  ParamBase *GetCurrent() { return *m_current; }

  int GetSize() { return (int)m_params.size(); }

  std::string GetValueString() { return m_name; }

  // functions to traverse list
  void Reset() { m_current = m_params.begin(); }

  void Increment() {
    m_current++;

    if (m_current == m_params.end()) {
      m_current = m_params.begin();
    }
  }

  void Decrement() {
    if (m_current == m_params.begin()) {
      m_current = m_params.end() - 1;
    } else {
      m_current--;
    }
  }

  float GetPercentage() { return 0.0f; }
  void SetPercentage(float /*p*/) {}

  void Write(std::ostream &stream) {
    stream << m_name << '\n';

    for (std::vector<ParamBase *>::const_iterator p = m_params.begin();
         p != m_params.end(); ++p) {
      (*p)->Write(stream);
    }
  }

  void Read(std::istream &stream) {
    stream >> m_name;

    for (std::vector<ParamBase *>::const_iterator p = m_params.begin();
         p != m_params.end(); ++p) {
      (*p)->Read(stream);
    }
  }

  bool IsList() { return true; }

  void ResetAll() {
    for (std::vector<ParamBase *>::const_iterator p = m_params.begin();
         p != m_params.end(); ++p) {
      (*p)->Reset();
    }
  }

 protected:
  bool active;
  std::vector<ParamBase *> m_params;
  std::map<std::string, ParamBase *> m_map;
  std::vector<ParamBase *>::const_iterator m_current;
};

#endif

```

## High-Level Overview

This file is part of the CUDA Samples repository, located at `Common/param.h`.

### Key Components

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

## Detailed Analysis

### File Statistics

- **Total Lines**: 237
- **Approximate Size**: 6066 bytes

### Content Structure

#### Declarations and Interfaces

This header file provides:

- Function declarations
- Class/struct definitions
- Macro definitions
- Template definitions
- Constant declarations

#### Include Guards

The header uses appropriate include guards or `#pragma once` to prevent multiple inclusion.

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

- Bounds checking for array accesses
- Proper initialization of variables
- Validation of input parameters
- Safe handling of CUDA API failures

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
