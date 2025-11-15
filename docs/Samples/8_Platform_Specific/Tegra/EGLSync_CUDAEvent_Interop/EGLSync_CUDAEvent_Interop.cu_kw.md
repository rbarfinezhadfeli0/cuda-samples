# Keywords: Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu
---

**Total Keywords**: 25

---

## B

### BLOCK_SIZE {#blocksize}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `#define BLOCK_SIZE 16

#define GL_READ  0
#define GL_WRITE 1
//---------------------------MACROS----`


## C

### CUDA_SAFE_CALL {#cudasafecall}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `#define CUDA_SAFE_CALL(call)                                                                        `

### CUDA_SAFE_CALL_NO_CLEANUP {#cudasafecallnocleanup}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `#define CUDA_SAFE_CALL_NO_CLEANUP(call, err)                                                        `


## F

### FAILURE {#failure}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `#define FAILURE 0
#define SUCCESS 1
#define WAIVED  2

#define BLOCK_SIZE 16

#define GL_READ  0
#de`


## G

### GL_READ {#glread}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `#define GL_READ  0
#define GL_WRITE 1
//---------------------------MACROS---------------------------`

### GL_SAFE_CALL {#glsafecall}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `#define GL_SAFE_CALL(call)                                                    \
    {               `

### GL_SAFE_CALL_NO_CLEANUP {#glsafecallnocleanup}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `#define GL_SAFE_CALL_NO_CLEANUP(call, err)                                       \
    {            `

### GL_WRITE {#glwrite}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `#define GL_WRITE 1
//---------------------------MACROS---------------------------------//

// Error-`


## M

### MAX_ITR {#maxitr}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `#define MAX_ITR 100

#define FAILURE 0
#define SUCCESS 1
#define WAIVED  2

#define BLOCK_SIZE 16

#`


## S

### SUCCESS {#success}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `#define SUCCESS 1
#define WAIVED  2

#define BLOCK_SIZE 16

#define GL_READ  0
#define GL_WRITE 1
//`

### SetupExtentions {#setupextentions}

- **Type**: identifier
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `) {
        printf("SetupExtentions failed \n");
      `


## W

### WAIVED {#waived}

- **Type**: macro
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `#define WAIVED  2

#define BLOCK_SIZE 16

#define GL_READ  0
#define GL_WRITE 1
//------------------`


## C

### checkSync {#checksync}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `void checkSync(int argc, char **argv)
{`

### checkSyncOnCPU {#checksynconcpu}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `void checkSyncOnCPU(void)
{`

### checkSyncOnGPU {#checksyncongpu}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `void checkSyncOnGPU(EGLDisplay dpy)
{`

### cleanup {#cleanup}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `void cleanup(int status)
{`

### cudaGetValueMismatch {#cudagetvaluemismatch}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `cudaError_t cudaGetValueMismatch()
{`


## E

### eglSetupExtensions {#eglsetupextensions}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `int eglSetupExtensions(void)
{`

### exitHandler {#exithandler}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `void exitHandler(void)
{`


## G

### getNumErrors {#getnumerrors}

- **Type**: cuda_kernel
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `__global__ void getNumErrors(`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `int main(int argc, char *argv[])
{`


## P

### parseCmdLine {#parsecmdline}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `int parseCmdLine(int argc, char **argv)
{`

### printStatus {#printstatus}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `void printStatus(int status)
{`

### printUsage {#printusage}

- **Type**: function
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `void printUsage(void)
{`


## V

### verify_and_update_kernel {#verifyandupdatekernel}

- **Type**: cuda_kernel
- **File**: [Samples/8_Platform_Specific/Tegra/EGLSync_CUDAEvent_Interop/EGLSync_CUDAEvent_Interop.cu](./EGLSync_CUDAEvent_Interop.cu_docs.md)
- **Context**: `__global__ void
verify_and_update_kernel(`

