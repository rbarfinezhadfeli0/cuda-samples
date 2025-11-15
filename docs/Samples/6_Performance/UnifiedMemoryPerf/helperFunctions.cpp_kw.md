# Keywords: Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp
---

**Total Keywords**: 18

---

## C

### CU_INIT_UUID {#cuinituuid}

- **Type**: macro
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `#define CU_INIT_UUID
#include <cmath>

#define UNITS_Time "ms"
#define UNITS_BW   "MB/s"
#define KB_`


## K

### KB_str {#kbstr}

- **Type**: macro
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `#define KB_str     "KB"
#define MB_str     "MB"

struct resultsData
{
    char                result`


## M

### MB_str {#mbstr}

- **Type**: macro
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `#define MB_str     "MB"

struct resultsData
{
    char                resultsName[64];
    struct te`


## U

### UNITS_BW {#unitsbw}

- **Type**: macro
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `#define UNITS_BW   "MB/s"
#define KB_str     "KB"
#define MB_str     "MB"

struct resultsData
{
    `

### UNITS_Time {#unitstime}

- **Type**: macro
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `#define UNITS_Time "ms"
#define UNITS_BW   "MB/s"
#define KB_str     "KB"
#define MB_str     "MB"

s`


## C

### calculateAverageAndStdDev {#calculateaverageandstddev}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `void calculateAverageAndStdDev(double *pAverage, double *pStdDev, double *allResults, unsigned int c`

### calculateStdDevBandwidth {#calculatestddevbandwidth}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `void calculateStdDevBandwidth(double *pStdDev, double *allResults, unsigned int count, unsigned long`

### compareDoubles {#comparedoubles}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `int compareDoubles(const void *ptr1, const void *ptr2) {`

### createAndInitTestResults {#createandinittestresults}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `void createAndInitTestResults(struct testResults **ptrResults,
                              const c`

### createResultDataAndAddToTestResults {#createresultdataandaddtotestresults}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `void createResultDataAndAddToTestResults(struct resultsData **ptrData,
                             `


## F

### findNumSizesToTest {#findnumsizestotest}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `int findNumSizesToTest(unsigned int minSize, unsigned int maxSize, unsigned int multiplier)
{`

### freeTestResultsAndAllResultsData {#freetestresultsandallresultsdata}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `void freeTestResultsAndAllResultsData(struct testResults *results)
{`


## G

### getTimeOrBandwidth {#gettimeorbandwidth}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `double getTimeOrBandwidth(double runTimeInMs, unsigned long size, bool getBandwidth)
{`


## P

### printAllResultsInVerboseMode {#printallresultsinverbosemode}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `void printAllResultsInVerboseMode(struct testResults *results, struct resultsData *data)
{`

### printResults {#printresults}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `void printResults(struct testResults *results, bool print_launch_transfer_results, bool print_std_de`

### printTimesInTableFormat {#printtimesintableformat}

- **Type**: function
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `void printTimesInTableFormat(struct testResults *results, struct resultsData *data, bool printAverag`


## R

### resultsData {#resultsdata}

- **Type**: type
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `struct resultsData`


## T

### testResults {#testresults}

- **Type**: type
- **File**: [Samples/6_Performance/UnifiedMemoryPerf/helperFunctions.cpp](./helperFunctions.cpp_docs.md)
- **Context**: `struct testResults`

