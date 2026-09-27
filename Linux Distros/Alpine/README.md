## Introduction
Uses [musl](https://en.wikipedia.org/wiki/Musl) which allow efficient [static linking](https://en.wikipedia.org/wiki/Static_library) and to have realtime-quality robustness by avoiding race conditions, internal failures on resource exhaustion, and various other bad worst-case behaviors present in existing implementations.  

**Musl** is a lightweight C standard library instead of the [standard GNU C Library (glibc)](https://www.gnu.org/software/libc/) used by distributions like Ubuntu and Debian.  

### Why Alpine Uses musl
- **Ultra-lightweight size**: The core library is under 1MB, helping keep Alpine base container images around 5MB to 8MB.  
- **Small attack surface**: Minimalism improves security and reduces resource consumption.  
- **Efficient linking**: Designed for simplicity and clean static and dynamic linking overhead.  