# Keywords: Samples/5_Domain_Specific/p2pBandwidthLatencyTest/p2pBandwidthLatencyTest.cu
---

**Total Keywords**: 11

---

## S

### StopWatchInterface {#stopwatchinterface}

- **Type**: identifier
- **File**: [Samples/5_Domain_Specific/p2pBandwidthLatencyTest/p2pBandwidthLatencyTest.cu](./p2pBandwidthLatencyTest.cu_docs.md)
- **Context**: `ag      = NULL;
    StopWatchInterface  *stopWatch = NULL;`


## C

### checkP2Paccess {#checkp2paccess}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/p2pBandwidthLatencyTest/p2pBandwidthLatencyTest.cu](./p2pBandwidthLatencyTest.cu_docs.md)
- **Context**: `void checkP2Paccess(int numGPUs)
{`

### copyp2p {#copyp2p}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/p2pBandwidthLatencyTest/p2pBandwidthLatencyTest.cu](./p2pBandwidthLatencyTest.cu_docs.md)
- **Context**: `__global__ void copyp2p(`

### cudaCheckError {#cudacheckerror}

- **Type**: macro
- **File**: [Samples/5_Domain_Specific/p2pBandwidthLatencyTest/p2pBandwidthLatencyTest.cu](./p2pBandwidthLatencyTest.cu_docs.md)
- **Context**: `#define cudaCheckError()                                                                     \
    {`


## D

### delay {#delay}

- **Type**: cuda_kernel
- **File**: [Samples/5_Domain_Specific/p2pBandwidthLatencyTest/p2pBandwidthLatencyTest.cu](./p2pBandwidthLatencyTest.cu_docs.md)
- **Context**: `__global__ void delay(`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/p2pBandwidthLatencyTest/p2pBandwidthLatencyTest.cu](./p2pBandwidthLatencyTest.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## O

### outputBandwidthMatrix {#outputbandwidthmatrix}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/p2pBandwidthLatencyTest/p2pBandwidthLatencyTest.cu](./p2pBandwidthLatencyTest.cu_docs.md)
- **Context**: `void outputBandwidthMatrix(int numElems, int numGPUs, bool p2p, P2PDataTransfer p2p_method)
{`

### outputBidirectionalBandwidthMatrix {#outputbidirectionalbandwidthmatrix}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/p2pBandwidthLatencyTest/p2pBandwidthLatencyTest.cu](./p2pBandwidthLatencyTest.cu_docs.md)
- **Context**: `void outputBidirectionalBandwidthMatrix(int numElems, int numGPUs, bool p2p)
{`

### outputLatencyMatrix {#outputlatencymatrix}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/p2pBandwidthLatencyTest/p2pBandwidthLatencyTest.cu](./p2pBandwidthLatencyTest.cu_docs.md)
- **Context**: `void outputLatencyMatrix(int numGPUs, bool p2p, P2PDataTransfer p2p_method)
{`


## P

### performP2PCopy {#performp2pcopy}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/p2pBandwidthLatencyTest/p2pBandwidthLatencyTest.cu](./p2pBandwidthLatencyTest.cu_docs.md)
- **Context**: `void performP2PCopy(int         *dest,
                    int          destDevice,
                `

### printHelp {#printhelp}

- **Type**: function
- **File**: [Samples/5_Domain_Specific/p2pBandwidthLatencyTest/p2pBandwidthLatencyTest.cu](./p2pBandwidthLatencyTest.cu_docs.md)
- **Context**: `void printHelp(void)
{`

