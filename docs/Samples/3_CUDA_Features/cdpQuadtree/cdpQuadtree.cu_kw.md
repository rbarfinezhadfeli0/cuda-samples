# Keywords: Samples/3_CUDA_Features/cdpQuadtree/cdpQuadtree.cu
---

**Total Keywords**: 15

---

## B

### Bounding_box {#boundingbox}

- **Type**: type
- **File**: [Samples/3_CUDA_Features/cdpQuadtree/cdpQuadtree.cu](./cdpQuadtree.cu_docs.md)
- **Context**: `class Bounding_box`


## P

### Parameters {#parameters}

- **Type**: type
- **File**: [Samples/3_CUDA_Features/cdpQuadtree/cdpQuadtree.cu](./cdpQuadtree.cu_docs.md)
- **Context**: `struct Parameters`

### Points {#points}

- **Type**: type
- **File**: [Samples/3_CUDA_Features/cdpQuadtree/cdpQuadtree.cu](./cdpQuadtree.cu_docs.md)
- **Context**: `class Points`


## Q

### Quadtree_node {#quadtreenode}

- **Type**: type
- **File**: [Samples/3_CUDA_Features/cdpQuadtree/cdpQuadtree.cu](./cdpQuadtree.cu_docs.md)
- **Context**: `class Quadtree_node`


## R

### Random_generator {#randomgenerator}

- **Type**: type
- **File**: [Samples/3_CUDA_Features/cdpQuadtree/cdpQuadtree.cu](./cdpQuadtree.cu_docs.md)
- **Context**: `struct Random_generator`


## B

### build_quadtree_kernel {#buildquadtreekernel}

- **Type**: cuda_kernel
- **File**: [Samples/3_CUDA_Features/cdpQuadtree/cdpQuadtree.cu](./cdpQuadtree.cu_docs.md)
- **Context**: `__global__ void build_quadtree_kernel(`


## C

### cdpQuadtree {#cdpquadtree}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpQuadtree/cdpQuadtree.cu](./cdpQuadtree.cu_docs.md)
- **Context**: `bool cdpQuadtree(int warp_size)
{`

### check_quadtree {#checkquadtree}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpQuadtree/cdpQuadtree.cu](./cdpQuadtree.cu_docs.md)
- **Context**: `bool check_quadtree(const Quadtree_node *nodes, int idx, int num_pts, Points *pts, Parameters params`


## H

### hash {#hash}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpQuadtree/cdpQuadtree.cu](./cdpQuadtree.cu_docs.md)
- **Context**: `int hash(unsigned int a)
    {`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpQuadtree/cdpQuadtree.cu](./cdpQuadtree.cu_docs.md)
- **Context**: `int main(int argc, char **argv)
{`


## S

### set {#set}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpQuadtree/cdpQuadtree.cu](./cdpQuadtree.cu_docs.md)
- **Context**: `void set(float min_x, float min_y, float max_x, float max_y)
    {`

### set_bounding_box {#setboundingbox}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpQuadtree/cdpQuadtree.cu](./cdpQuadtree.cu_docs.md)
- **Context**: `void set_bounding_box(float min_x, float min_y, float max_x, float max_y)
    {`

### set_id {#setid}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpQuadtree/cdpQuadtree.cu](./cdpQuadtree.cu_docs.md)
- **Context**: `void set_id(int new_id) {`

### set_point {#setpoint}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpQuadtree/cdpQuadtree.cu](./cdpQuadtree.cu_docs.md)
- **Context**: `void set_point(int idx, const float2 &p)
    {`

### set_range {#setrange}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/cdpQuadtree/cdpQuadtree.cu](./cdpQuadtree.cu_docs.md)
- **Context**: `void set_range(int begin, int end)
    {`

