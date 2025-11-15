# Keyword Map for Samples/3_CUDA_Features/bf16TensorCoreGemm/bf16TensorCoreGemm.cu

## File Information

- **File Path**: `Samples/3_CUDA_Features/bf16TensorCoreGemm/bf16TensorCoreGemm.cu`
- **Documentation**: [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md)
- **Source**: [View Source](../../Samples/3_CUDA_Features/bf16TensorCoreGemm/bf16TensorCoreGemm.cu)

## Extracted Keywords

This file contains 58 keywords and identifiers:

### Keywords by Category


#### Cuda Keyword

- **__global__**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **__host__**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **__shared__**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **block**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **cudaFree**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **cudaMalloc**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **cudaMemcpy**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **grid**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **kernel**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **thread**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **warp**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)

#### Filename

- **bf16TensorCoreGemm**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)

#### Function

- **BLOCK_COL_TILES**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **BLOCK_ROW_TILES**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **CHUNK_COPY_LINES_PER_WARP**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **CHUNK_COPY_LINE_LANES**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **CHUNK_LINE_BYTES**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **DAMAGES**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **GEMM**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **K_GLOBAL**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **M_GLOBAL**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **N_GLOBAL**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **SHMEM_OFFSET**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **SHMEM_STRIDE**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **THREADS_PER_BLOCK**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **Volta**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **WARP_COPY_BYTES**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **__nv_bfloat16**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **checkCudaErrors**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **checkKernelErrors**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **compute_bf16gemm**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **compute_bf16gemm_async_copy**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **copy**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **correct**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **for**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **init_host_matrices**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **main**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **matMultiplyOnHost**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **printf**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **simple_wmma_bf16gemm**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)

#### Macro

- **BLOCK_COL_WARPS**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **BLOCK_ROW_WARPS**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **CHUNK_K**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **CPU_DEBUG**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **C_LAYOUT**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **GLOBAL_MEM_STRIDE**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **K**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **K_TILES**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **M**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **M_TILES**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **N**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **N_TILES**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **SHARED_MEMORY_LIMIT_64K**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **SKEW_BF16**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **WARPS_PER_BLOCK**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **WARP_COL_TILES**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **WARP_ROW_TILES**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)
- **WARP_SIZE**: Defined in this file - See [bf16TensorCoreGemm.cu_docs.md](bf16TensorCoreGemm.cu_docs.md#detailed-analysis)


## Keyword → Documentation Mapping

Each keyword above links back to the detailed documentation for this file, where you can find:

- Complete context for the keyword
- Implementation details
- Usage examples
- Related concepts

## Search Index

You can search for any of the above keywords to find this file in the global keyword index.

---

*This keyword map was automatically generated.*
