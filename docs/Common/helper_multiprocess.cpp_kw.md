# Keywords: Common/helper_multiprocess.cpp
---

**Total Keywords**: 41

---

## C

### CloseHandle {#closehandle}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `o->shmHandle) {
    CloseHandle(info->shmHandle);
 `

### CreateFile {#createfile}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `DLE hFile =
        CreateFile(TEXT(childSlotName.`

### CreateFileMapping {#createfilemapping}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `  info->shmHandle = CreateFileMapping(INVALID_HANDLE_VALU`

### CreateMailslot {#createmailslot}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `empBuf);

  hSlot = CreateMailslot((LPSTR)clientSlotNa`

### CreateProcess {#createprocess}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `++;
  }

  status = CreateProcess(app, LPSTR(arg_stri`


## D

### DuplicateHandle {#duplicatehandle}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `'s space
      if (!DuplicateHandle(GetCurrentProcess()`


## G

### GetCurrentProcess {#getcurrentprocess}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `f (!DuplicateHandle(GetCurrentProcess(), shareableHandles`

### GetCurrentProcessId {#getcurrentprocessid}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `pBuf[20];
  _itoa_s(GetCurrentProcessId(), tempBuf, 10);
  `

### GetExitCodeProcess {#getexitcodeprocess}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `ocess, INFINITE);
  GetExitCodeProcess(process->hProcess, `

### GetLastError {#getlasterror}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: ` == 0) {
    return GetLastError();
  }

  info->add`


## M

### MapViewOfFile {#mapviewoffile}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `  }

  info->addr = MapViewOfFile(info->shmHandle, FI`


## O

### OpenFileMapping {#openfilemapping}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `  info->shmHandle = OpenFileMapping(FILE_MAP_ALL_ACCESS`

### OpenProcess {#openprocess}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: ` hProcess =
        OpenProcess(PROCESS_DUP_HANDLE,`


## R

### ReadFile {#readfile}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `cbRead = 0;

  if (!ReadFile(handle->hMailslot[0`


## S

### ShareableHandle {#shareablehandle}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `(ipcHandle *handle, ShareableHandle *shHandle) {
  stru`

### SlotName {#slotname}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `process ids.
LPTSTR SlotName = (LPTSTR)TEXT("\\\`


## U

### UnmapViewOfFile {#unmapviewoffile}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: ` (info->addr) {
    UnmapViewOfFile(info->addr);
  }
  `


## W

### WaitForSingleObject {#waitforsingleobject}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `  DWORD exitCode;
  WaitForSingleObject(process->hProcess, `

### WriteFile {#writefile}

- **Type**: identifier
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `ritten;

  result = WriteFile(mailslot, data, (DW`


## C

### cmsghdr {#cmsghdr}

- **Type**: type
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `struct cmsghdr`


## I

### iovec {#iovec}

- **Type**: type
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `struct iovec`

### ipcCloseShareableHandle {#ipccloseshareablehandle}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `int ipcCloseShareableHandle(ShareableHandle shHandle) {`

### ipcCloseSocket {#ipcclosesocket}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `int ipcCloseSocket(ipcHandle *handle) {`

### ipcCreateSocket {#ipccreatesocket}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `int ipcCreateSocket(ipcHandle *&handle, const char *name,
                    const std::vector<Proc`

### ipcOpenSocket {#ipcopensocket}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `int ipcOpenSocket(ipcHandle *&handle) {`

### ipcRecvData {#ipcrecvdata}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `int ipcRecvData(ipcHandle *handle, void *data, size_t sz) {`

### ipcRecvDataFromClient {#ipcrecvdatafromclient}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `int ipcRecvDataFromClient(ipcHandle *serverHandle, void *data, size_t size) {`

### ipcRecvShareableHandle {#ipcrecvshareablehandle}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `int ipcRecvShareableHandle(ipcHandle *handle, ShareableHandle *shHandle) {`

### ipcRecvShareableHandles {#ipcrecvshareablehandles}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `int ipcRecvShareableHandles(ipcHandle *handle,
                            std::vector<ShareableHand`

### ipcSendData {#ipcsenddata}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `int ipcSendData(HANDLE mailslot, const void *data, size_t sz) {`

### ipcSendDataToServer {#ipcsenddatatoserver}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `int ipcSendDataToServer(ipcHandle *handle, const char *serverName,
                        const voi`

### ipcSendShareableHandle {#ipcsendshareablehandle}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `int ipcSendShareableHandle(ipcHandle *handle,
                           const std::vector<Shareable`

### ipcSendShareableHandles {#ipcsendshareablehandles}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `int ipcSendShareableHandles(
    ipcHandle *handle, const std::vector<ShareableHandle> &shareableHan`


## M

### msghdr {#msghdr}

- **Type**: type
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `struct msghdr`


## S

### sharedMemoryClose {#sharedmemoryclose}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `void sharedMemoryClose(sharedMemoryInfo *info) {`

### sharedMemoryCreate {#sharedmemorycreate}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `int sharedMemoryCreate(const char *name, size_t sz, sharedMemoryInfo *info) {`

### sharedMemoryOpen {#sharedmemoryopen}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `int sharedMemoryOpen(const char *name, size_t sz, sharedMemoryInfo *info) {`

### sockaddr {#sockaddr}

- **Type**: type
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `struct sockaddr`

### sockaddr_un {#sockaddrun}

- **Type**: type
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `struct sockaddr_un`

### spawnProcess {#spawnprocess}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `int spawnProcess(Process *process, const char *app, char *const *args) {`


## W

### waitProcess {#waitprocess}

- **Type**: function
- **File**: [Common/helper_multiprocess.cpp](./helper_multiprocess.cpp_docs.md)
- **Context**: `int waitProcess(Process *process) {`

