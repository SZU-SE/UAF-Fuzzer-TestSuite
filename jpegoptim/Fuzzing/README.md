### jpegoptim
- Bug type: double free
- CVE ID: [CVE-2018-11416](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-11416)
- Download: https://github.com/tjko/jpegoptim
```
git clone https://github.com/tjko/jpegoptim.git
git checkout d23abf2c59692e0e3638ce8c89d98a3628c686b7
```
- Reproduce: `./jpegoptim $POC`
- ASAN dumps the backtrace:
```
=================================================================
==11432==ERROR: AddressSanitizer: attempting double-free on 0x62d000000400 in thread T0:
    #0 0x4decf8 in __interceptor_cfree.localalias.0 /home/hjwang/Tools/llvm-6.0.1/projects/compiler-rt/lib/asan/asan_malloc_linux.cc:76
    #1 0x51f003 in main /home/hjwang/UAF_Objects/jpegoptim_asan/jpegoptim.c:899:3
    #2 0x7fcbac77382f in __libc_start_main /build/glibc-LK5gWL/glibc-2.23/csu/../csu/libc-start.c:291
    #3 0x41ac88 in _start (/home/hjwang/UAF_Objects/jpegoptim_asan/build/bin/jpegoptim+0x41ac88)

0x62d000000400 is located 0 bytes inside of 39485-byte region [0x62d000000400,0x62d000009e3d)
freed by thread T0 here:
    #0 0x4df320 in realloc /home/hjwang/Tools/llvm-6.0.1/projects/compiler-rt/lib/asan/asan_malloc_linux.cc:107
    #1 0x51fa95 in jpeg_memory_empty_output_buffer /home/hjwang/UAF_Objects/jpegoptim_asan/jpegdest.c:59:12
    #2 0x7fcbad3682bd in emit_byte /home/hjwang/UAF_Objects/GraphicsMagick-1.3.26_Fuzzing/jpeg-9c/jcmarker.c:117
    #3 0x60f00000012f  (<unknown module>)
    #4 0x7fcbad37a49f  (/usr/local/lib/libjpeg.so.9+0x1e49f)

previously allocated by thread T0 here:
    #0 0x4deeb8 in malloc /home/hjwang/Tools/llvm-6.0.1/projects/compiler-rt/lib/asan/asan_malloc_linux.cc:88
    #1 0x51c804 in main /home/hjwang/UAF_Objects/jpegoptim_asan/jpegoptim.c:681:14
    #2 0x7fcbac77382f in __libc_start_main /build/glibc-LK5gWL/glibc-2.23/csu/../csu/libc-start.c:291

SUMMARY: AddressSanitizer: double-free /home/hjwang/Tools/llvm-6.0.1/projects/compiler-rt/lib/asan/asan_malloc_linux.cc:76 in __interceptor_cfree.localalias.0
==11432==ABORTING
```