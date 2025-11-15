# Keywords: Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp
---

**Total Keywords**: 23

---

## C

### CanAccessPeer {#canaccesspeer}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `remove devices from CanAccessPeer
            for (in`

### ConvertStringSecurityDescriptorToSecurityDescriptorA {#convertstringsecuritydescriptortosecuritydescriptora}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `      BOOL result = ConvertStringSecurityDescriptorToSecurityDescriptorA(sddl, SDDL_REVISION`


## D

### DATA_BUF_SIZE {#databufsize}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `#define DATA_BUF_SIZE        4ULL * 1024ULL * 1024ULL

static const char ipcName[] = "memmap_ipc_pip`


## G

### GetLastError {#getlasterror}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `or Failed! (%d)\n", GetLastError());
        }

    `


## I

### InitializeObjectAttributes {#initializeobjectattributes}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `
        }

        InitializeObjectAttributes(&objAttributes, NUL`

### InterlockedAdd {#interlockedadd}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `_atomic_add32(a, x) InterlockedAdd((volatile LONG *)a,`


## M

### MAX_DEVICES {#maxdevices}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `#define MAX_DEVICES (32)

#define PROCESSES_PER_DEVICE 1
#define DATA_BUF_SIZE        4ULL * 1024ULL`


## P

### PROCESSES_PER_DEVICE {#processesperdevice}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `#define PROCESSES_PER_DEVICE 1
#define DATA_BUF_SIZE        4ULL * 1024ULL * 1024ULL

static const c`

### PTX_FILE {#ptxfile}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `#define PTX_FILE "memMapIpc_kernel32.ptx"
#endif

// `ipcHandleTypeFlag` specifies the platform spec`


## S

### ShareableHandle {#shareablehandle}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `        std::vector<ShareableHandle>              &shar`

### ShareableHandles {#shareablehandles}

- **Type**: identifier
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `   // close all the ShareableHandles.
    for (int i = 0`


## B

### barrierWait {#barrierwait}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `void barrierWait(volatile int *barrier, volatile int *sense, unsigned int n)
{`


## C

### childProcess {#childprocess}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `void childProcess(int devId, int id, char **argv)
{`

### cpu_atomic_add32 {#cpuatomicadd32}

- **Type**: macro
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `#define cpu_atomic_add32(a, x) InterlockedAdd((volatile LONG *)a, x)
#else
#error Unsupported system`


## F

### findModulePath {#findmodulepath}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `inline findModulePath(const char *module_file, string &module_path, char **argv, string &ptx_source)`


## G

### getDefaultSecurityDescriptor {#getdefaultsecuritydescriptor}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `void getDefaultSecurityDescriptor(CUmemAllocationProp *prop)
{`


## M

### main {#main}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `int main(int argc, char **argv)
{`

### memMapAllocateAndExportMemory {#memmapallocateandexportmemory}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `void memMapAllocateAndExportMemory(unsigned char                              backingDevice,
       `

### memMapGetDeviceFunction {#memmapgetdevicefunction}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `void memMapGetDeviceFunction(char **argv)
{`

### memMapImportAndMapMemory {#memmapimportandmapmemory}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `void memMapImportAndMapMemory(CUdeviceptr                   d_ptr,
                                 `

### memMapUnmapAndFreeMemory {#memmapunmapandfreememory}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `void memMapUnmapAndFreeMemory(CUdeviceptr dptr, size_t size)
{`


## P

### parentProcess {#parentprocess}

- **Type**: function
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `void parentProcess(char *app)
{`


## S

### shmStruct_st {#shmstructst}

- **Type**: type
- **File**: [Samples/3_CUDA_Features/memMapIPCDrv/memMapIpc.cpp](./memMapIpc.cpp_docs.md)
- **Context**: `struct shmStruct_st`

