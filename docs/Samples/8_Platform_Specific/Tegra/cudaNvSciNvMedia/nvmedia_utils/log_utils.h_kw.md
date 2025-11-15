# Keywords: Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.h
---

**Total Keywords**: 11

---

## L

### LINE_INFO {#lineinfo}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.h](./log_utils.h_docs.md)
- **Context**: `#define LINE_INFO     __FUNCTION__, __LINE__
#define LOG_DBG(...)  LogLevelMessage(LEVEL_DBG, LINE_I`

### LOG_DBG {#logdbg}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.h](./log_utils.h_docs.md)
- **Context**: `#define LOG_DBG(...)  LogLevelMessage(LEVEL_DBG, LINE_INFO, __VA_ARGS__)
#define LOG_INFO(...) LogLe`

### LOG_INFO {#loginfo}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.h](./log_utils.h_docs.md)
- **Context**: `#define LOG_INFO(...) LogLevelMessage(LEVEL_INFO, LINE_INFO, __VA_ARGS__)
#define LOG_WARN(...) LogL`

### LOG_WARN {#logwarn}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.h](./log_utils.h_docs.md)
- **Context**: `#define LOG_WARN(...) LogLevelMessage(LEVEL_WARN, LINE_INFO, __VA_ARGS__)

    //  SetLogLevel
    /`

### LogLevel {#loglevel}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.h](./log_utils.h_docs.md)
- **Context**: `<stdio.h>

    enum LogLevel {
        LEVEL_ERR`

### LogLevelMessage {#loglevelmessage}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.h](./log_utils.h_docs.md)
- **Context**: `efine LOG_DBG(...)  LogLevelMessage(LEVEL_DBG, LINE_INF`

### LogStyle {#logstyle}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.h](./log_utils.h_docs.md)
- **Context**: `3,
    };

    enum LogStyle { LOG_STYLE_NORMAL `


## S

### SetLogFile {#setlogfile}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.h](./log_utils.h_docs.md)
- **Context**: `le style);

    //  SetLogFile
    //
    //    Se`

### SetLogLevel {#setloglevel}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.h](./log_utils.h_docs.md)
- **Context**: `VA_ARGS__)

    //  SetLogLevel
    //
    //    Se`

### SetLogStyle {#setlogstyle}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.h](./log_utils.h_docs.md)
- **Context**: `el level);

    //  SetLogStyle
    //
    //    Se`


## _

### _NVMEDIA_TEST_LOG_UTILS_H_ {#nvmediatestlogutilsh}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.h](./log_utils.h_docs.md)
- **Context**: `#define _NVMEDIA_TEST_LOG_UTILS_H_

#ifdef __cplusplus
extern "C"
{
#endif

#include <stdarg.h>
#inc`

