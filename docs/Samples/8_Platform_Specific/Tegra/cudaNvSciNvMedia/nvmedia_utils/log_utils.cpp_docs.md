# Documentation: Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp
---
## File Metadata
- **Path**: `Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp`
- **Filename**: `log_utils.cpp`
- **Language**: cpp
- **Size**: 4518 bytes
- **Lines**: 155
- **Generated**: 2025-11-15 12:53:50 UTC

---
## Original Source
```cpp
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


#include <stdio.h>
#include <string.h>
#ifdef NVMEDIA_ANDROID
#define LOG_TAG    "nvmedia_common"
#define LOG_NDEBUG 1
#include <utils/Log.h>
#endif
#ifdef NVMEDIA_QNX
#include <sys/slog.h>
#endif

#include "log_utils.h"

#ifdef NVMEDIA_QNX
#define NV_SLOGCODE 0xAAAA
#endif
#define MAX_STATS_LEN 500

#define LOG_BUFFER_BYTES 1024

static enum LogLevel msg_level = LEVEL_ERR;
static enum LogStyle msg_style = LOG_STYLE_NORMAL;
static FILE         *msg_file  = NULL;

void SetLogLevel(enum LogLevel level)
{
    if (level > LEVEL_DBG)
        return;

    msg_level = level;
}

void SetLogStyle(enum LogStyle style)
{
    if (style > LOG_STYLE_FUNCTION_LINE)
        return;

    msg_style = style;
}

void SetLogFile(FILE *logFileHandle)
{
    if (!logFileHandle)
        return;

    msg_file = logFileHandle;
}

void LogLevelMessage(enum LogLevel level, const char *functionName, int lineNumber, const char *format, ...)
{
    va_list ap;
    char    str[LOG_BUFFER_BYTES] = {
        '\0',
    };
    FILE *logFile = msg_file ? msg_file : stdout;

    if (level > msg_level)
        return;

#ifndef NVMEDIA_ANDROID
    /** In the case of Android ADB log, if LOG_TAG is defined,
     * before 'Log.h' is included in source file,
     * LOG_TAG is automatically concatenated at the beginning of log message,
     * so, we don't copy 'nvmedia: ' into 'str'.
     */
    strcpy(str, "nvmedia: ");

    /** As LOG_TAG is concatednated, log level is also automatically concatenated,
     * by calling different ADB log function such as ALOGE(for eror log message),
     * ALOGW(for warning log message).
     */
    switch (level) {
    case LEVEL_ERR:
        strcat(str, "ERROR: ");
        break;
    case LEVEL_WARN:
        strcat(str, "WARNING: ");
        break;
    case LEVEL_INFO:
    case LEVEL_DBG:
        // Empty
        break;
    }
#endif

    va_start(ap, format);
    vsnprintf(str + strlen(str), sizeof(str) - strlen(str), format, ap);

    if (msg_style == LOG_STYLE_NORMAL) {
        // Add trailing new line char
        if (strlen(str) && str[strlen(str) - 1] != '\n')
            strcat(str, "\n");
    }
    else if (msg_style == LOG_STYLE_FUNCTION_LINE) {
        // Remove trailing new line char
        if (strlen(str) && str[strlen(str) - 1] == '\n')
            str[strlen(str) - 1] = 0;

        // Add function and line info
        snprintf(str + +strlen(str), sizeof(str) - strlen(str), " at %s():%d\n", functionName, lineNumber);
    }

#ifdef NVMEDIA_ANDROID
    switch (msg_level) {
    case LEVEL_ERR:
        ALOGE("%s", str);
        break;
    case LEVEL_WARN:
        ALOGW("%s", str);
        break;
    case LEVEL_INFO:
        ALOGI("%s", str);
        break;
    case LEVEL_DBG:
        ALOGD("%s", str);
        break;
    }
#else
    fprintf(logFile, "%s", str);
#endif
#ifdef NVMEDIA_QNX
    /* send to system logger */
    slogf(_SLOG_SETCODE(NV_SLOGCODE, 0), _SLOG_ERROR, str);
#endif
    va_end(ap);
}

```

---
## High-Level Overview
This file is a cpp source file with 6 function(s) in the CUDA Samples repository.

**Dependencies**: 5 included headers/modules


---
## Detailed Walkthrough
### Includes / Imports
- `stdio.h`
- `string.h`
- `utils/Log.h`
- `sys/slog.h`
- `log_utils.h`

### Preprocessor Definitions
- **LOG_TAG**: `"nvmedia_common"`
- **LOG_NDEBUG**: `1`
- **NV_SLOGCODE**: `0xAAAA`
- **MAX_STATS_LEN**: `500`
- **LOG_BUFFER_BYTES**: `1024`

### Functions
#### `void SetLogLevel(enum LogLevel level)`
- Function in Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp

#### `void SetLogStyle(enum LogStyle style)`
- Function in Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp

#### `void SetLogFile(FILE *logFileHandle)`
- Function in Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp

#### `void LogLevelMessage(enum LogLevel level, const char *functionName, int lineNumber, const char *format, ...)`
- Function in Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp

#### `else if(msg_style == LOG_STYLE_FUNCTION_LINE)`
- Function in Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp

#### `NVMEDIA_ANDROID switch(msg_level)`
- Function in Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp


---
## Usage Examples
This is a C/C++ source file. Typical usage involves:
1. Compiling with gcc/g++ or compatible compiler
2. Linking with required libraries
3. Executing the resulting binary


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

