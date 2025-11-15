# Keywords: Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/config_parser.h
---

**Total Keywords**: 14

---

## C

### ConfigParamsMap {#configparamsmap}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/config_parser.h](./config_parser.h_docs.md)
- **Context**: ` sectionType;
    } ConfigParamsMap;

    NvMediaStatus`

### ConfigParser_DisplayParams {#configparserdisplayparams}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/config_parser.h](./config_parser.h_docs.md)
- **Context**: `;
    NvMediaStatus ConfigParser_DisplayParams(ConfigParamsMap *pa`

### ConfigParser_GetSectionIndexByName {#configparsergetsectionindexbyname}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/config_parser.h](./config_parser.h_docs.md)
- **Context**: `;
    NvMediaStatus ConfigParser_GetSectionIndexByName(SectionMap *section`

### ConfigParser_GetSectionIndexByType {#configparsergetsectionindexbytype}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/config_parser.h](./config_parser.h_docs.md)
- **Context**: `  NvMediaStatus
    ConfigParser_GetSectionIndexByType(SectionMap *section`

### ConfigParser_InitParamsMap {#configparserinitparamsmap}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/config_parser.h](./config_parser.h_docs.md)
- **Context**: `

    NvMediaStatus ConfigParser_InitParamsMap(ConfigParamsMap *pa`

### ConfigParser_ParseFile {#configparserparsefile}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/config_parser.h](./config_parser.h_docs.md)
- **Context**: `  NvMediaStatus
    ConfigParser_ParseFile(ConfigParamsMap *pa`

### ConfigParser_ValidateParams {#configparservalidateparams}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/config_parser.h](./config_parser.h_docs.md)
- **Context**: `;
    NvMediaStatus ConfigParser_ValidateParams(ConfigParamsMap *pa`


## L

### LimitsType {#limitstype}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/config_parser.h](./config_parser.h_docs.md)
- **Context**: `, LIMITS_BOTH = 2 } LimitsType;

    typedef enum `


## M

### MAX_ITEMS_TO_PARSE {#maxitemstoparse}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/config_parser.h](./config_parser.h_docs.md)
- **Context**: `#define MAX_ITEMS_TO_PARSE 10000

    typedef enum _ParamType {
        TYPE_UINT = 0,
        TYPE_`


## N

### NvMediaStatus {#nvmediastatus}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/config_parser.h](./config_parser.h_docs.md)
- **Context**: `nfigParamsMap;

    NvMediaStatus ConfigParser_InitPa`


## P

### ParamType {#paramtype}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/config_parser.h](./config_parser.h_docs.md)
- **Context**: `   TYPE_SHORT
    } ParamType;

    typedef enum `


## S

### SectionMap {#sectionmap}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/config_parser.h](./config_parser.h_docs.md)
- **Context**: `sizeOfStruct;
    } SectionMap;

    typedef struc`

### SectionType {#sectiontype}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/config_parser.h](./config_parser.h_docs.md)
- **Context**: `N_2DPROCESSOR
    } SectionType;

    typedef struc`


## _

### _NVMEDIA_TEST_CONFIG_PARSER_H_ {#nvmediatestconfigparserh}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/cudaNvSciNvMedia/nvmedia_utils/config_parser.h](./config_parser.h_docs.md)
- **Context**: `#define _NVMEDIA_TEST_CONFIG_PARSER_H_

#ifdef __cplusplus
extern "C"
{
#endif

#include <ctype.h>
#`

