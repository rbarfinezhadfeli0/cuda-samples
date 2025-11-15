# Keywords: Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu
---

**Total Keywords**: 22

---

## A

### AlgorithmStatus {#algorithmstatus}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `/ Run steps
        AlgorithmStatus status;

        tr`


## D

### DeviceMemoryPool {#devicememorypool}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `class DeviceMemoryPool`


## G

### Graph {#graph}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `struct Graph`


## I

### IsGreaterEqualThan {#isgreaterequalthan}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `ints + edgesCount_, IsGreaterEqualThan<uint>(newVerticesCo`


## L

### Level {#level}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `class Level`

### LevelsIterator {#levelsiterator}

- **Type**: identifier
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `unt);

        for (LevelsIterator level = levels_.rbe`


## M

### MemoryPoolsCollection {#memorypoolscollection}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `struct MemoryPoolsCollection`


## P

### Pyramid {#pyramid}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `class Pyramid`


## S

### SegmentationTreeBuilder {#segmentationtreebuilder}

- **Type**: type
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `class SegmentationTreeBuilder`


## A

### addLevel {#addlevel}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `void addLevel(uint                     totalSuperNodes,
                  uint                     t`


## B

### buildFromDeviceData {#buildfromdevicedata}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `void buildFromDeviceData(thrust::device_ptr<uint> superVerticesOffsets, thrust::device_ptr<uint> ver`

### buildGraph {#buildgraph}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `void buildGraph(const vector<uchar3> &image, uint width, uint height, Graph &graph)
{`


## C

### calculateThreadsDistribution {#calculatethreadsdistribution}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `void calculateThreadsDistribution(uint totalElements, uint &blocksCount, uint &threadsPerBlockCount)`


## D

### distance {#distance}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `float distance(const uchar3 &first, const uchar3 &second)
{`


## I

### initalizeData {#initalizedata}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `void initalizeData(const Graph &graph, MemoryPoolsCollection &pools)
    {`

### invokeStep {#invokestep}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `AlgorithmStatus invokeStep(MemoryPoolsCollection &pools, Pyramid &segmentations)
    {`


## L

### loadImage {#loadimage}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `int loadImage(const char *filename, const char *executablePath, vector<uchar3> &data, uint &width, u`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### myrand {#myrand}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `int myrand(void)
{`


## P

### printMemoryUsage {#printmemoryusage}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `void printMemoryUsage()
    {`

### put {#put}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `void put(const thrust::device_ptr<T> &ptr) {`


## R

### run {#run}

- **Type**: function
- **File**: [Samples/2_Concepts_and_Techniques/segmentationTreeThrust/segmentationTree.cu](./segmentationTree.cu_docs.md)
- **Context**: `float run(const Graph &graph, Pyramid &segmentations)
    {`

