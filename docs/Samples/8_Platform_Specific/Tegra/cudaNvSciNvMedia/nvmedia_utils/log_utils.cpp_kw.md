# Keywords: Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp
---

**Total Keywords**: 12

---

## L

### LOG_BUFFER_BYTES {#logbufferbytes}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp](./log_utils.cpp_docs.md)
- **Context**: `#define LOG_BUFFER_BYTES 1024

static enum LogLevel msg_level = LEVEL_ERR;
static enum LogStyle msg_`

### LOG_NDEBUG {#logndebug}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp](./log_utils.cpp_docs.md)
- **Context**: `#define LOG_NDEBUG 1
#include <utils/Log.h>
#endif
#ifdef NVMEDIA_QNX
#include <sys/slog.h>
#endif

`

### LOG_TAG {#logtag}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp](./log_utils.cpp_docs.md)
- **Context**: `#define LOG_TAG    "nvmedia_common"
#define LOG_NDEBUG 1
#include <utils/Log.h>
#endif
#ifdef NVMEDI`

### LogLevel {#loglevel}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp](./log_utils.cpp_docs.md)
- **Context**: `S 1024

static enum LogLevel msg_level = LEVEL_E`

### LogLevelMessage {#loglevelmessage}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp](./log_utils.cpp_docs.md)
- **Context**: `void LogLevelMessage(enum LogLevel level, const char *functionName, int lineNumber, const char *form`

### LogStyle {#logstyle}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp](./log_utils.cpp_docs.md)
- **Context**: `EL_ERR;
static enum LogStyle msg_style = LOG_STY`


## M

### MAX_STATS_LEN {#maxstatslen}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp](./log_utils.cpp_docs.md)
- **Context**: `#define MAX_STATS_LEN 500

#define LOG_BUFFER_BYTES 1024

static enum LogLevel msg_level = LEVEL_ERR`


## N

### NV_SLOGCODE {#nvslogcode}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp](./log_utils.cpp_docs.md)
- **Context**: `#define NV_SLOGCODE 0xAAAA
#endif
#define MAX_STATS_LEN 500

#define LOG_BUFFER_BYTES 1024

static e`


## S

### SetLogFile {#setlogfile}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp](./log_utils.cpp_docs.md)
- **Context**: `void SetLogFile(FILE *logFileHandle)
{`

### SetLogLevel {#setloglevel}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp](./log_utils.cpp_docs.md)
- **Context**: `void SetLogLevel(enum LogLevel level)
{`

### SetLogStyle {#setlogstyle}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp](./log_utils.cpp_docs.md)
- **Context**: `void SetLogStyle(enum LogStyle style)
{`

### switch {#switch}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/log_utils.cpp](./log_utils.cpp_docs.md)
- **Context**: `NVMEDIA_ANDROID
    switch (msg_level) {`

