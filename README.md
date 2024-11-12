# fileops

A no_std file operations library providing filesystem and I/O functionality.

This crate implements core filesystem operations for a no_std environment, including:

File descriptor management

+ File and directory operations
+ Path manipulation
+ Permission handling
+ Device file support

Core Features

+ Standard file operations (open, read, write, seek)
+ Directory operations (mkdir, rmdir, chdir)
+ Symbolic and hard link support
+ Permission and ownership management
+ Special device files (/dev) support

## Examples

```rust
#![no_std]
#![no_main]

#[macro_use]
extern crate axlog2;

use core::panic::PanicInfo;

#[no_mangle]
pub extern "Rust" fn runtime_main(cpu_id: usize, _dtb_pa: usize) {
    axlog2::init("debug");
    info!("[rt_fileops]: ...");
    fileops::init(cpu_id, _dtb_pa);

    info!("[rt_fileops]: ok!");
    axhal::misc::terminate();
}

#[panic_handler]
pub fn panic(info: &PanicInfo) -> ! {
    arch_boot::panic(info)
}
```

## Constants

### `AT_EMPTY_PATH`

```rust
pub const AT_EMPTY_PATH: usize = 0x1000;
```

Flag to operate on file descriptor itself

### `AT_FDCWD`

```rust
pub const AT_FDCWD: usize = _; // 18_446_744_073_709_551_516usize
```

Special file descriptor representing current working directory

### `AT_REMOVEDIR`

```rust
pub const AT_REMOVEDIR: usize = 0x200;
```

Flag to remove directory instead of file

## Functions

### `chdir`

```rust
pub fn chdir(path: &str) -> usize
```

Changes current working directory

### `console_on_rootfs`

```rust
pub fn console_on_rootfs() -> LinuxResult
```

Sets up console devices for standard I/O

### `do_open`

```rust
pub fn do_open(filename: &str, flags: i32) -> LinuxResult<FileRef>
```

Opens a file

### `dup`

```rust
pub fn dup(fd: usize) -> usize
```

Duplicates a file descriptor

### `dup3`

```rust
pub fn dup3(oldfd: usize, newfd: usize, flags: usize) -> usize
```

Duplicates file descriptor to specific number

### `faccessat`

```rust
pub fn faccessat(dfd: usize, path: &str) -> usize
```

Checks if file is accessible

### `fallocate`

```rust
pub fn fallocate(
    fd: usize,
    mode: usize,
    offset: usize,
    len: usize
) -> LinuxResult<usize>
```

Pre-allocates space for a file

### `fchmod`

```rust
pub fn fchmod(fd: usize, mode: i32) -> LinuxResult<usize>
```

Changes file mode/permissions relative to a directory file descriptor

### `fchmodat`

```rust
pub fn fchmodat(
    dfd: usize,
    filename: &str,
    mode: i32,
    _flags: usize
) -> LinuxResult<usize>
```

Changes file mode/permissions relative to a directory file descriptor

### `fchown`

```rust
pub fn fchown(fd: usize, uid: u32, gid: u32) -> LinuxResult<usize>
```

Changes ownership of an open file descriptor

### `fchownat`

```rust
pub fn fchownat(
    dfd: usize,
    filename: &str,
    uid: u32,
    gid: u32,
    flags: usize
) -> LinuxResult<usize>
```

Changes ownership of a file relative to a directory file descriptor

### `fcntl`

```rust
pub fn fcntl(fd: usize, cmd: usize, udata: usize) -> usize
```

Manipulates file descriptor

### `filetype`

```rust
pub fn filetype(fname: &str) -> LinuxResult<VfsNodeType>
```

Gets file type from path

### `fstatat`

```rust
pub fn fstatat(
    dfd: usize,
    path: usize,
    statbuf_ptr: usize,
    flags: usize
) -> usize
```

Gets information about a file

### `ftruncate`

```rust
pub fn ftruncate(fd: usize, length: usize) -> usize
```

Truncates a file to specified length

### `getcwd`

```rust
pub fn getcwd(buf: &mut [u8]) -> usize
```

Gets current working directory

### `getdents64`

```rust
pub fn getdents64(fd: usize, dirp: usize, count: usize) -> usize
```

Gets directory entries

### `init`

```rust
pub fn init(cpu_id: usize, dtb_pa: usize)
```

Initializes file operations subsystem

### `ioctl`

```rust
pub fn ioctl(fd: usize, request: usize, udata: usize) -> LinuxResult<usize>
```

Performs device-specific operations

### `linkat`

```rust
pub fn linkat(
    olddfd: usize,
    oldpath: &str,
    newdfd: usize,
    newpath: &str,
    flags: usize
) -> LinuxResult<usize>
```

Creates a hard link

### `loop_init`

```rust
pub fn loop_init() -> LinuxResult
```

Initializes loop devices

### `lseek`

```rust
pub fn lseek(fd: usize, offset: usize, whence: usize) -> usize
```

Moves file pointer

### `mkdirat`

```rust
pub fn mkdirat(dfd: usize, pathname: &str, mode: usize) -> usize
```

Creates a directory

### `mknodat`

```rust
pub fn mknodat(dfd: usize, filename: &str, mode: usize, dev: usize) -> usize
```

Creates a special file node

### `mount`

```rust
pub fn mount(
    fsname: &str,
    dir: &str,
    fstype: &str,
    flags: usize,
    data: usize
) -> LinuxResult<usize>
```

Mounts a filesystem

### `openat`

```rust
pub fn openat(
    dfd: usize,
    filename: &str,
    flags: usize,
    mode: usize
) -> AxResult<File>
```

Opens a file relative to a directory file descriptor

### `pipe2`

```rust
pub fn pipe2(fds: usize, flags: usize) -> LinuxResult
```

Creates a pipe

### `pread64`

```rust
pub fn pread64(fd: usize, ubuf: &mut [u8], offset: usize) -> LinuxResult<usize>
```

Reads from a file descriptor at given offset

### `read`

```rust
pub fn read(fd: usize, ubuf: &mut [u8]) -> LinuxResult<usize>
```

Reads from a file descriptor

### `readlinkat`

```rust
pub fn readlinkat(
    dfd: usize,
    filename: &str,
    buf: usize,
    size: usize
) -> LinuxResult<usize>
```

Reads value of symbolic link

### `register_file`

```rust
pub fn register_file(file: AxResult<File>, flags: usize) -> usize
```

Associates file descriptor with provided file

### `sendfile`

```rust
pub fn sendfile(
    out_fd: usize,
    in_fd: usize,
    offset: usize,
    count: usize
) -> usize
```

Transfers data between file descriptors

### `statfs`

```rust
pub fn statfs(path: &str, buf: usize) -> usize
```

Gets filesystem statistics

### `symlinkat`

```rust
pub fn symlinkat(target: &str, newdfd: usize, linkpath: &str) -> usize
```

Creates a symbolic link

### `unlinkat`

```rust
pub fn unlinkat(dfd: usize, path: &str, flags: usize) -> usize
```

Deletes a file or directory entry

### `unregister_file`

```rust
pub fn unregister_file(fd: usize) -> LinuxResult<Arc<Mutex<File>>>
```

Removes file descriptor and returns associated file

### `utimensat`

```rust
pub fn utimensat(
    dfd: usize,
    filename: &str,
    times: usize,
    flags: usize
) -> usize
```

Updates file timestamps

### `write`

```rust
pub fn write(fd: usize, ubuf: &[u8]) -> LinuxResult<usize>
```

Writes to a file descriptor

### `writev`

```rust
pub fn writev(fd: usize, iov_array: &[iovec]) -> usize
```

Writes data from multiple buffers (vectored I/O)
