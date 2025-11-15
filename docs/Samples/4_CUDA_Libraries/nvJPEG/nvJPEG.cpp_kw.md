# Keywords: Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp
---

**Total Keywords**: 17

---

## F

### FileData {#filedata}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `<std::vector<char>> FileData;

struct decode_par`

### FileNames {#filenames}

- **Type**: identifier
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `<std::string>       FileNames;
typedef std::vecto`


## C

### create_decoupled_api_handles {#createdecoupledapihandles}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `void create_decoupled_api_handles(decode_params_t &params)
{`


## D

### decode_images {#decodeimages}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `int decode_images(const FileData             &img_data,
                  const std::vector<size_t> `

### decode_params_t {#decodeparamst}

- **Type**: type
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `struct decode_params_t`

### destroy_decoupled_api_handles {#destroydecoupledapihandles}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `void destroy_decoupled_api_handles(decode_params_t &params)
{`

### dev_free {#devfree}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `int dev_free(void *p) {`

### dev_malloc {#devmalloc}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `int dev_malloc(void **p, size_t s) {`


## F

### findParamIndex {#findparamindex}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `int findParamIndex(const char **argv, int argc, const char *parm)
{`


## H

### host_free {#hostfree}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `int host_free(void *p) {`

### host_malloc {#hostmalloc}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `int host_malloc(void **p, size_t s, unsigned int f) {`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `int main(int argc, const char *argv[])
{`


## P

### prepare_buffers {#preparebuffers}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `int prepare_buffers(FileData                   &file_data,
                    std::vector<size_t>  `

### process_images {#processimages}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `double process_images(FileNames &image_names, decode_params_t &params, double &total)
{`


## R

### read_next_batch {#readnextbatch}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `int read_next_batch(FileNames           &image_names,
                    int                  batch`

### release_buffers {#releasebuffers}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `void release_buffers(std::vector<nvjpegImage_t> &ibuf)
{`


## W

### write_images {#writeimages}

- **Type**: function
- **File**: [Samples/4_CUDA_Libraries/nvJPEG/nvJPEG.cpp](./nvJPEG.cpp_docs.md)
- **Context**: `void write_images(std::vector<nvjpegImage_t> &iout,
                  std::vector<int>           &wi`

