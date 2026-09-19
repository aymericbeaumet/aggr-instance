---
title: '`zig cc`: a Powerful Drop-In Replacement for GCC/Clang'
link: https://andrewkelley.me/post/zig-cc-powerful-drop-in-replacement-gcc-clang.html
source: andrewkelley-me
published: 2020-03-24T14:39:47Z
updated: 2020-03-24T14:39:47Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: '`zig cc`: a Powerful Drop-In Replacement for GCC/Clang If you have heard of Zig before, you may know it as a promising new programming language which is ambitiously trying to overthrow C as the de-facto systems language. But did you know that it also can straight up compile C code? This has been possible for a while, and you can see some examples of this on the home page. What''s new is that the zig cc sub-command is available, and it supports the same options as Clang, which, in turn, supports the same options as GCC. Now, I''m sure you''re feeling pretty skeptical right about now, so let me hook you real quick before I get into the juicy details. Clang and GCC cannot do this: andy@ark ~/tmp> cat hello.c #include <stdio.h> int main(int argc, char **argv) { fprintf(stderr, "Hello, World!\n"); return 0; } andy@ark ~/tmp> clang -o hello.exe hello.c -target x86_64-windows-gnu clang-7: warning: argument unused during compilation: ''--gcc-toolchain=/nix/store/ificps9si1nvz85f9xa7gjd9h6r5lzg6-gcc-9.2.0'' [-Wunused-command-line-argument] /nix/store/7bhi29ainf5rjrk7k7wyhndyskzyhsxh-binutils-2.31.1/bin/ld: unrecognised emulation mode: i386pep Supported emulations: elf_x86_64 elf32_x86_64 elf_i386 elf_iamcu elf_l1om elf_k1om clang-7: error: linker command failed with exit code 1 (use -v to see invocation) andy@ark ~/tmp> clang -o hello hello.c -target mipsel-linux-musl In file included from hello.c:1: In file included from /nix/store/8pp3i3hcp7bv0f8jllzqq7gcp9dbzvp9-glibc-2.27-dev/include/stdio.h:27: In file included from /nix/store/8pp3i3hcp7bv0f8jllzqq7gcp9dbzvp9-glibc-2.27-dev/include/bits/libc-header-start.h:33: In file included from /nix/store/8pp3i3hcp7bv0f8jllzqq7gcp9dbzvp9-glibc-2.27-dev/include/features.h:452: /nix/store/8pp3i3hcp7bv0f8jllzqq7gcp9dbzvp9-glibc-2.27-dev/include/gnu/stubs.h:7:11: fatal error: ''gnu/stubs-32.h'' file not found # include <gnu/stubs-32.h> ^~~~~~~~~~~~~~~~ 1 error generated. andy@ark ~/tmp> clang -o hello hello.c -target aarch64-linux-gnu In file included from hello.c:1: In file included from /nix/store/8pp3i3hcp7bv0f8jllzqq7gcp9dbzvp9-glibc-2.27-dev/include/stdio.h:27: In file included from /nix/store/8pp3i3hcp7bv0f8jllzqq7gcp9dbzvp9-glibc-2.27-dev/include/bits/libc-header-start.h:33: In file included from /nix/store/8pp3i3hcp7bv0f8jllzqq7gcp9dbzvp9-glibc-2.27-dev/include/features.h:452: /nix/store/8pp3i3hcp7bv0f8jllzqq7gcp9dbzvp9-glibc-2.27-dev/include/gnu/stubs.h:7:11: fatal error: ''gnu/stubs-32.h'' file not found # include <gnu/stubs-32.h> ^~~~~~~~~~~~~~~~ 1 error generated. `zig cc` can: andy@ark ~/tmp> zig cc -o hello.exe hello.c -target x86_64-windows-gnu andy@ark ~/tmp> wine64 hello.exe Hello, World! andy@ark ~/tmp> zig cc -o hello hello.c -target mipsel-linux-musl andy@ark ~/tmp> qemu-mipsel ./hello Hello, World! andy@ark ~/tmp> zig cc -o hello hello.c -target aarch64-linux-gnu andy@ark ~/tmp> qemu-aarch64 -L ~/Downloads/glibc/multi-2.31/install/glibcs/aarch64-linux-gnu ./hello Hello, World! Features of `zig cc` zig cc is not the main purpose of the Zig project. It merely exposes the already-existing capabilities of the Zig compiler via a small frontend layer that parses C compiler options. Install simply by unzipping a tarball Zig is an open source project, and of course can be built and installed from source the usual way. However, the Zig project also has tarballs available on the download page. You can download a 45 MiB tarball, unpack it, and you''re done. You can even have multiple versions at the same time, no problem. Here, rather than downloading the x86_64-linux version, which matches the computer I am currently using, I''ll download the Windows version and run it in Wine to show how simple installation is: andy@ark ~/tmp> wget --quiet https://ziglang.org/builds/zig-windows-x86_64-0.5.0+13d04f996.zip andy@ark ~/tmp> unzip -q zig-windows-x86_64-0.5.0+13d04f996.zip andy@ark ~/tmp> wine64 ./zig-windows-x86_64-0.5.0+13d04f996/zig.exe cc -o hello hello.c -target x86_64-linux andy@ark ~/tmp> ./hello Hello, World! Take a moment to appreciate what just happened here - I downloaded a Windows build of Zig, ran it in Wine, using it to cross compile for Linux, and then ran the binary natively. Computers are fun! Compare this to downloading Clang, which has 380 MiB Linux-distribution-specific tarballs. Zig''s Linux tarballs are fully statically linked, and therefore work correctly on all Linux distributions. The size difference here comes because the Clang tarball ships with more utilities than a C compiler, as well as pre-compiled static libraries for both LLVM and Clang. Zig does not ship with any pre-compiled libraries; instead it ships with source code, and builds what it needs on-the-fly. Caching System The Zig compiler uses a sophisticated caching system to avoid needlessly rebuilding artifacts. I carefully designed this caching system to make optimal use of the file system while maintaining correct semantics - which is trickier than you might think! The caching system uses a combination of hashing inputs and checking the fstat values of file paths, while being mindful of mtime granularity. This makes it avoid needlessly hashing files, while at the same time detecting when a modified file has the same contents. It always has correct behavior, whether the file system has nanosecond mtime granularity, second granularity, always sets mtime to zero, or anything in between. You can find a detailed description of the caching system in the 0.4.0 release notes. zig cc makes this caching system available when compiling C code. For simple enough projects, this obviates the need for a Makefile or other build system. andy@ark ~/tmp> cat foo.c #include <stdio.h> #include "another_file.c" int main(int argc, char **argv) { #include "printf_many_times.c" } andy@ark ~/tmp> cat another_file.c void another(void) {} andy@ark ~/tmp> time zig cc -c foo.c 0.12 andy@ark ~/tmp> time zig cc -c foo.c 0.01 andy@ark ~/tmp> touch another_file.c andy@ark ~/tmp> time zig cc -c foo.c 0.01 andy@ark ~/tmp> echo "/* add a comment */" >>another_file.c andy@ark ~/tmp> time zig cc -c foo.c 0.12 andy@ark ~/tmp> time zig cc -c foo.c 0.01 Here you can see the caching system is smart enough to find dependencies that are included with the preprocessor, and smart enough to avoid a full rebuild when the mtime of another_file.c was updated. One last thing before I move on. I want to point out that this caching system is not some fluffy bloated feature - rather it is an absolutely critical component to making cross-compiling work in a usable manner. As we''ll see below, other compilers ship with pre-compiled, target-specific binaries, while Zig ships with source code only and cross-compiles on-the-fly, caching the result. Cross Compiling I have carefully designed Zig since the very beginning to treat cross compilation as a first class use case. Now that the zig cc frontend is available, it brings these capabilities to C code. I showed you above cross-compiling some simple "Hello, World!" programs. But now let''s try a real-world C project. Let''s try LuaJIT! [~/Downloads]$ git clone https://github.com/LuaJIT/LuaJIT [~/Downloads]$ cd LuaJIT [~/Downloads/LuaJIT]$ ls COPYRIGHT doc dynasm etc Makefile README src OK so it uses standard Makefiles. Here we go, first let''s make sure it works natively with zig cc. [~/Downloads/LuaJIT]$ export CC="zig cc" [~/Downloads/LuaJIT]$ make CC="$CC" ==== Building LuaJIT 2.1.0-beta3 ==== make -C src make[1]: Entering directory ''/home/andy/Downloads/LuaJIT/src'' HOSTCC host/minilua.o HOSTLINK host/minilua DYNASM host/buildvm_arch.h HOSTCC host/buildvm.o HOSTCC host/buildvm_asm.o HOSTCC host/buildvm_peobj.o HOSTCC host/buildvm_lib.o HOSTCC host/buildvm_fold.o HOSTLINK host/buildvm BUILDVM lj_vm.S ASM lj_vm.o CC lj_gc.o BUILDVM lj_ffdef.h CC lj_err.o CC lj_char.o BUILDVM lj_bcdef.h CC lj_bc.o CC lj_obj.o CC lj_buf.o CC lj_str.o CC lj_tab.o CC lj_func.o CC lj_udata.o CC lj_meta.o CC lj_debug.o CC lj_state.o CC lj_dispatch.o CC lj_vmevent.o CC lj_vmmath.o CC lj_strscan.o CC lj_strfmt.o CC lj_strfmt_num.o CC lj_api.o CC lj_profile.o CC lj_lex.o CC lj_parse.o CC lj_bcread.o CC lj_bcwrite.o CC lj_load.o CC lj_ir.o CC lj_opt_mem.o BUILDVM lj_folddef.h CC lj_opt_fold.o CC lj_opt_narrow.o CC lj_opt_dce.o CC lj_opt_loop.o CC lj_opt_split.o CC lj_opt_sink.o CC lj_mcode.o CC lj_snap.o CC lj_record.o CC lj_crecord.o BUILDVM lj_recdef.h CC lj_ffrecord.o CC lj_asm.o CC lj_trace.o CC lj_gdbjit.o CC lj_ctype.o CC lj_cdata.o CC lj_cconv.o CC lj_ccall.o CC lj_ccallback.o CC lj_carith.o CC lj_clib.o CC lj_cparse.o CC lj_lib.o CC lj_alloc.o CC lib_aux.o BUILDVM lj_libdef.h CC lib_base.o CC lib_math.o CC lib_bit.o CC lib_string.o CC lib_table.o CC lib_io.o CC lib_os.o CC lib_package.o CC lib_debug.o CC lib_jit.o CC lib_ffi.o CC lib_init.o AR libluajit.a CC luajit.o BUILDVM jit/vmdef.lua DYNLINK libluajit.so LINK luajit warning: unsupported linker arg: -E OK Successfully built LuaJIT make[1]: Leaving directory ''/home/andy/Downloads/LuaJIT/src'' ==== Successfully built LuaJIT 2.1.0-beta3 ==== [~/Downloads/LuaJIT]$ ls COPYRIGHT doc dynasm etc Makefile README src [~/Downloads/LuaJIT]$ ./src/ host/ jit/ libluajit.so luajit zig-cache/ [~/Downloads/LuaJIT]$ ./src/luajit LuaJIT 2.1.0-beta3 -- Copyright (C) 2005-2020 Mike Pall. http://luajit.org/ JIT: ON SSE2 SSE3 SSE4.1 BMI2 fold cse dce fwd dse narrow loop abc sink fuse > print(3 + 4) 7 > OK so that worked. Now for the real test - can we make it cross compile? [~/Downloads/LuaJIT]$ git clean -xfdq [~/Downloads/LuaJIT]$ export CC="zig cc -target aarch64-linux-gnu" [~/Downloads/LuaJIT]$ export HOST_CC="zig cc" [~/Downloads/LuaJIT]$ make CC="$CC" HOST_CC="$HOST_CC" TARGET_STRIP="echo" ==== Building LuaJIT 2.1.0-beta3 ==== make -C src make[1]: Entering directory ''/home/andy/Downloads/LuaJIT/src'' HOSTCC host/minilua.o HOSTLINK host/minilua DYNASM host/buildvm_arch.h HOSTCC host/buildvm.o HOSTCC host/buildvm_asm.o HOSTCC host/buildvm_peobj.o HOSTCC host/buildvm_lib.o HOSTCC host/buildvm_fold.o HOSTLINK host/buildvm BUILDVM lj_vm.S ASM lj_vm.o CC lj_gc.o BUILDVM lj_ffdef.h CC lj_err.o CC lj_char.o BUILDVM lj_bcdef.h CC lj_bc.o CC lj_obj.o CC lj_buf.o CC lj_str.o CC lj_tab.o CC lj_func.o CC lj_udata.o CC lj_meta.o CC lj_debug.o CC lj_state.o CC lj_dispatch.o CC lj_vmevent.o CC lj_vmmath.o CC lj_strscan.o CC lj_strfmt.o CC lj_strfmt_num.o CC lj_api.o CC lj_profile.o CC lj_lex.o CC lj_parse.o CC lj_bcread.o CC lj_bcwrite.o CC lj_load.o CC lj_ir.o CC lj_opt_mem.o BUILDVM lj_folddef.h CC lj_opt_fold.o CC lj_opt_narrow.o CC lj_opt_dce.o CC lj_opt_loop.o CC lj_opt_split.o CC lj_opt_sink.o CC lj_mcode.o CC lj_snap.o CC lj_record.o CC lj_crecord.o BUILDVM lj_recdef.h CC lj_ffrecord.o CC lj_asm.o CC lj_trace.o CC lj_gdbjit.o CC lj_ctype.o CC lj_cdata.o CC lj_cconv.o CC lj_ccall.o CC lj_ccallback.o CC lj_carith.o CC lj_clib.o CC lj_cparse.o CC lj_lib.o CC lj_alloc.o CC lib_aux.o BUILDVM lj_libdef.h CC lib_base.o CC lib_math.o CC lib_bit.o CC lib_string.o CC lib_table.o CC lib_io.o CC lib_os.o CC lib_package.o CC lib_debug.o CC lib_jit.o CC lib_ffi.o CC lib_init.o AR libluajit.a CC luajit.o BUILDVM jit/vmdef.lua DYNLINK libluajit.so libluajit.so LINK luajit warning: unsupported linker arg: -E luajit OK Successfully built LuaJIT make[1]: Leaving directory ''/home/andy/Downloads/LuaJIT/src'' ==== Successfully built LuaJIT 2.1.0-beta3 ==== [~/Downloads/LuaJIT]$ file ./src/luajit ./src/luajit: ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-aarch64.so.1, for GNU/Linux 2.0.0, with debug_info, not stripped It worked! Will it run in QEMU though? [~/Downloads/LuaJIT]$ qemu-aarch64 -L ~/Downloads/glibc/multi-2.31/install/glibcs/aarch64-linux-gnu ./src/luajit LuaJIT 2.1.0-beta3 -- Copyright (C) 2005-2020 Mike Pall. http://luajit.org/ JIT: ON fold cse dce fwd dse narrow loop abc sink fuse > print(4 + 3) 7 > Amazing. QEMU never fails to impress me. Before we move on, I want to show one more thing. You can see above, in order to run the foreign-architecture binary, I had to pass -L ~/Downloads/glibc/multi-2.31/install/glibcs/aarch64-linux-gnu. This is due to the binary being dynamically linked. You can confirm this with the output from file above where it says: dynamically linked, interpreter /lib/ld-linux-aarch64.so.1 Often, when cross-compiling, it is useful to make a static binary. In the case of Linux, for example, this will make the resulting binary able to run on any Linux distribution, rather than only ones with a hard-coded glibc dynamic linker path of /lib/ld-linux-aarch64.so.1. We can accomplish this by targeting musl rather than glibc: [~/Downloads/LuaJIT]$ git clean -qxfd [~/Downloads/LuaJIT]$ export CC="zig cc -target aarch64-linux-musl" [~/Downloads/LuaJIT]$ make CC="$CC" CXX="$CXX" HOST_CC="$HOST_CC" TARGET_STRIP="echo" ==== Building LuaJIT 2.1.0-beta3 ==== (same output) ==== Successfully built LuaJIT 2.1.0-beta3 ==== [~/Downloads/LuaJIT]$ file src/luajit src/luajit: ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), statically linked, not stripped [~/Downloads/LuaJIT]$ qemu-aarch64 ./src/luajit LuaJIT 2.1.0-beta3 -- Copyright (C) 2005-2020 Mike Pall. http://luajit.org/ JIT: ON fold cse dce fwd dse narrow loop abc sink fuse > print(11 + 22) 33 Here you can see the file command reported statically linked, and in the qemu command, the -L parameter was not needed. Use Cases of `zig cc` Alright, so I''ve given you a taste of what zig cc can do, but now I will list explicitly what I consider to be the use cases: Experimentation Sometimes you just want a tool that you can use to try out different things. It can quickly answer questions such as "What assembly does this code generate on MIPS vs ARM?". The widely popular Compiler Explorer serves this purpose. zig cc provides a lightweight tool which can also answer questions such as, "What happens if I swap out glibc for musl?" and "How big is this executable when cross-compiled for Windows?". Here''s me using Zig to quickly find out what the maximum UDP packet size is on Linux. Since Zig is so easy to install - and it actually works everywhere without patches, even Linux distributions such as NixOS - it can often be a more convenient tool for running quick C test programs on your computer. At the time of this writing, LLVM 10 was just released two hours ago. It will take days or weeks for it to become available in various system package managers. But you can already download a master branch build of Zig and play with the new features of Clang/LLVM 10. For example, improved RISC-V support! andy@ark ~/tmp> zig cc -o hello hello.c -target riscv64-linux-musl andy@ark ~/tmp> qemu-riscv64 ./hello Hello, World! Bundling a C compiler as part of a larger project With Zig tarballs weighing in at under 45 MiB, zero system dependencies, no configuration, and MIT license, it makes for an ideal candidate when you need to bundle a C compiler along with another project. For example, maybe you have a programming language that compiles to C. Zig is an obvious choice for what C compiler to ship with your language. Or maybe you want to make a batteries-included IDE that ships with a compiler. Lightweight alternative to a cross compilation environment If you''re trying to build something with a large dependency tree, you''ll probably want to use a full cross compilation environment, such as mxe.cc or musl.cc. But if you don''t need such a sledgehammer, zig cc could be a useful alternative, especially if your goal is to compile for N different targets. Consider that musl.cc lists different tarballs for each architecture, each weighing in at roughly 85 MiB. Meanwhile Zig weighs in at 45 MiB and it supports all those architectures, plus glibc and Windows. An alternative to installing MSVC on Windows You could spend days - literally! - waiting for Microsoft Visual Studio to install, or you could install Zig and VS Code in a matter of minutes. Under the Hood If zig cc is built on top of Clang, why doesn''t Clang just do this? What exactly is Zig doing on top of Clang to make this work? The answer is, a lot, actually. I''ll go over how it works here. compiler-rt compiler-rt is a library that provides "polyfill" implementations of language-supported features when the target does not have machine code instructions for it. For example, compiler-rt has the function __muldi3 to perform signed 64-bit integer multiplication on architectures that do not have a 64-bit wide integer multiplication instruction. In the GNU world, compiler-rt is named libgcc. Most C compilers ship with this library pre-built for the target. For example, on an Ubuntu (Bionic) system, with the build-essential package installed, you can find this at /lib/x86_64-linux-gnu/libgcc_s.so.1. If you download clang+llvm-9.0.1-x86_64-linux-gnu-ubuntu-16.04.tar.xz and take a look around, clang actually does not even ship with compiler-rt. Instead, it relies on the system libgcc noted above. This is one reason that this tarball is Ubuntu-specific and does not work on other Linux distributions, FreeBSD''s Linuxulator, or WSL, which have system files in different locations. Zig''s strategy with compiler-rt is that we have our own implementation of this library, written in Zig. Most of it is ported from LLVM''s compiler-rt project, but we also have some of our own improvements on top of this. Anyway, rather than depending on system compiler-rt being installed, or shipping a pre-compiled library, Zig ships its compiler-rt in source form, and lazily builds compiler-rt for the compilation target, and then caches the result using the caching system discussed above. Zig''s compiler-rt is not yet complete. However, completing it is a prerequisite for releasing Zig version 1.0.0. libc When C code calls printf, printf has to be implemented somewhere, and that somewhere is libc. Some operating systems, such as FreeBSD and macOS, have a designated system libc, and it is the kernel syscall interface. On others, such as Windows and Linux, libc is optional, and therefore there are multiple options of which libc to use, if any. As of the time of this writing, Zig can provide libcs for the following targets: andy@ark ~> zig targets | jq .libc [ "aarch64_be-linux-gnu", "aarch64_be-linux-musl", "aarch64_be-windows-gnu", "aarch64-linux-gnu", "aarch64-linux-musl", "aarch64-windows-gnu", "armeb-linux-gnueabi", "armeb-linux-gnueabihf", "armeb-linux-musleabi", "armeb-linux-musleabihf", "armeb-windows-gnu", "arm-linux-gnueabi", "arm-linux-gnueabihf", "arm-linux-musleabi", "arm-linux-musleabihf", "arm-windows-gnu", "i386-linux-gnu", "i386-linux-musl", "i386-windows-gnu", "mips64el-linux-gnuabi64", "mips64el-linux-gnuabin32", "mips64el-linux-musl", "mips64-linux-gnuabi64", "mips64-linux-gnuabin32", "mips64-linux-musl", "mipsel-linux-gnu", "mipsel-linux-musl", "mips-linux-gnu", "mips-linux-musl", "powerpc64le-linux-gnu", "powerpc64le-linux-musl", "powerpc64-linux-gnu", "powerpc64-linux-musl", "powerpc-linux-gnu", "powerpc-linux-musl", "riscv64-linux-gnu", "riscv64-linux-musl", "s390x-linux-gnu", "s390x-linux-musl", "sparc-linux-gnu", "sparcv9-linux-gnu", "wasm32-freestanding-musl", "x86_64-linux-gnu", "x86_64-linux-gnux32", "x86_64-linux-musl", "x86_64-windows-gnu" ] In order to provide libc on these targets, Zig ships with a subset of the source files for these projects: musl v1.2.0 mingw-w64 v7.0.0 glibc 2.31 For each libc, there is a process for upgrading to a new release. This process is a sort of pre-processing step. We still end up with source files, but we de-duplicate non-multi-arch source files into multi-arch source files. glibc glibc is the most involved. The first step is building glibc for every target that it supports, which takes upwards of 24 hours and 74 GiB of disk space. From here, the process_headers tool inspects all the header files from all the targets, and identifies which files are the same across all targets, and which header files are target-specific. They are then sorted into the corresponding directories in Zig''s source tree, in: lib/libc/include/generic-glibc/ lib/libc/include/$ARCH-linux-$ABI/ (there are multiple of these directories) Additionally, Linux header files are not included in glibc, and so the same process is applied to Linux header files, with the directories: lib/libc/include/any-linux-any/ lib/libc/include/$ARCH-linux-any/ That takes care of the header files, but now we have the problem of dynamic linking against glibc, without touching any system files. For this, we have the update_glibc tool. Given the path to the glibc source directory, it finds all the .abilist text files and uses them to produce 3 simple but crucial files: vers.txt - the list of all glibc versions. fns.txt - the list of all symbols that glibc provides, followed by the library it appears in (for example libm, libpthread, libc, librt). abi.txt - for each target, for each function, tells which versions of glibc, if any, it appears in. Together, these files amount to only 192 KB (27 KB gzipped), and they allow Zig to target any version of glibc. Yes, I did not make a typo there. Zig can target any of the 42 versions of glibc for any of the architectures listed above. I''ll show you: andy@ark ~/tmp> cat rand.zig const std = @import("std"); pub fn main() anyerror!void { var buf: [10]u8 = undefined; _ = std.c.getrandom(&buf, buf.len, 0); std.debug.warn("random bytes: {x}\n", .{buf}); } andy@ark ~/tmp> zig build-exe rand.zig -lc -target native-native-gnu.2.25 andy@ark ~/tmp> ./rand random bytes: e2059382afb599ea6d29 andy@ark ~/tmp> zig build-exe rand.zig -lc -target native-native-gnu.2.24 lld: error: undefined symbol: getrandom >>> referenced by rand.zig:5 (/home/andy/tmp/rand.zig:5) >>> ./rand.o:(main.0) Sure enough, if you look at the man page for getrandom, it says: Support was added to glibc in version 2.25. When no explicit glibc version is requested, and the target OS is the native (host) OS, Zig detects the native glibc version by inspecting the Zig executable''s own dynamically linked libraries, looking for glibc, and checking the version. It turns out you can look for libc.so.6 and then readlink on that, and it will look something like libc-2.27.so. When this strategy does not work, Zig looks at /usr/bin/env, looking for the same thing. Since this file path is hard-coded into countless shebang lines, it''s a pretty safe bet to find out the dynamic linker path and glibc version (if any) of the native system! zig cc currently does not provide a way to choose a specific glibc version (because C compilers do not provide a way), and so Zig chooses the native version for compiling natively, and the default (2.17) for cross-compiling. However, I''m sure this problem can be solved, even when using zig cc. For example, maybe it could support an environment variable, or simply introduce an extra command line option that does not conflict with any Clang options. When you request a certain version of glibc, Zig uses those text files noted above to create dummy .so files to link against, which contain exactly the correct set of symbols (with appropriate name mangling) based on the requested version. The symbols will be resolved at runtime, by the dynamic linker on the target platform. In this way, most of libc in the glibc case resides on the target file system. But not all of it! There are still the "C runtime start files": Scrt1.o crti.o crtn.o These are statically compiled into every binary that dynamically links glibc, and their ABI is therefore Very Very Stable. And so, Zig bundles a small subset of glibc''s source files needed to build these object files from source for every target. The total size of this comes out to 1.4 MiB (252 KB gzipped). I do think there is some room for improvement here, but I digress. There are a couple of patches to this small subset of glibc source files, which simplify them to avoid including too many .h files, since the end result that we need is some bare bones object files, and not all of glibc. And finally, we certainly do not ship the build system of glibc with Zig! I manually inspected, audited, and analyzed glibc''s build system, and then by hand wrote code in the Zig compiler which hooks into Zig''s caching system and performs a minimal build of only these start files, as needed. musl The process for preparing musl to ship with Zig is much simpler by comparison. It still involves building musl for every target architecture that it supports, but in this case only the install-headers target has to be run, and it takes less than a minute, even to do it for all targets. The same process_headers tool tool used for glibc headers is used on the musl headers: lib/libc/include/generic-musl/ lib/libc/include/$ARCH-linux-$ABI/ (there are multiple of these directories) Unlike glibc, musl supports building statically. Zig currently assumes a static libc when musl is chosen, and does not support dynamically linking against musl, although that could potentially be added in the future. And so for musl, zig actually bundles most - but still not all - of musl''s source files. Everything in arch, crt, compat, src, and include gets copied in. Again much like glibc, I carefully studied musl''s build system, and then hand-coded logic in the Zig compiler to build these source files. In musl''s case it is simpler - just a bit of logic having to do with the file extension, and whether to override files with an architecture-specific file. The only file that needs to be patched (by hand) is version.h, which is normally generated during the configure phase in musl''s build system. I really appreciate Rich Felker''s efforts to make musl simple to utilize in this way, and he has been incredibly helpful in the #musl IRC channel when I ask questions. I proudly sponsor Rich Felker for $150/month. mingw-w64 mingw-w64 was an absolute joy to support in Zig. The beautiful thing about this project is that they have already been transitioning into having one set of header files that applies to all architectures (using #ifdefs only where needed). One set of header files is sufficient to support all four architectures: arm, aarch64, x86, and x86_64. So for updating headers, all we have to do is build mingw-w64, then: mv $INSTALLPREFIX/include $ZIGSRC/lib/libc/include/any-windows-any After doing this for all 3 libcs, the libc/include directory looks like this: aarch64_be-linux-any i386-linux-musl powerpc-linux-any aarch64_be-linux-gnu mips64el-linux-any powerpc-linux-gnu aarch64-linux-any mips64el-linux-gnuabi64 powerpc-linux-musl aarch64-linux-gnu mips64el-linux-gnuabin32 riscv32-linux-any aarch64-linux-musl mips64-linux-any riscv64-linux-any any-linux-any mips64-linux-gnuabi64 riscv64-linux-gnu any-windows-any mips64-linux-gnuabin32 riscv64-linux-musl armeb-linux-any mips64-linux-musl s390x-linux-any armeb-linux-gnueabi mipsel-linux-any s390x-linux-gnu armeb-linux-gnueabihf mipsel-linux-gnu s390x-linux-musl arm-linux-any mips-linux-any sparc-linux-gnu arm-linux-gnueabi mips-linux-gnu sparcv9-linux-gnu arm-linux-gnueabihf mips-linux-musl x86_64-linux-any arm-linux-musl powerpc64le-linux-any x86_64-linux-gnu generic-glibc powerpc64le-linux-gnu x86_64-linux-gnux32 generic-musl powerpc64-linux-any x86_64-linux-musl i386-linux-any powerpc64-linux-gnu i386-linux-gnu powerpc64-linux-musl When Zig generates a C command line to send to clang, it puts the appropriate include paths using -I depending on the target. For example, if the target is aarch64-linux-musl, then the following command line parameters are appended: -I$LIB/libc/include/aarch64-linux-musl -I$LIB/libc/include/aarch64-linux-any -I$LIB/libc/include/generic-musl Anyway back to mingw-w64. Again, Zig includes a subset of source files from mingw-w64 with a few patches applied to make things compile successfully. The Zig compiler code that builds mingw-w64 from source files emulates only the parts of the build system that are needed for this subset. This includes preprocessing .def.in files to get .def files, and then in-turn using LLD to generate .lib files from the .def files, which allows Zig to provide .lib files for any Windows DLL, such as kernel32.dll or even opengl32.dll. Invoking Clang Without a System Dependency Since Zig already links against Clang libraries for the translate-c feature, it was not much more cost to expose the main() entry point from Zig. So that''s exactly what we do: llvm-project/clang/tools/driver/driver.cpp is copied to $ZIGGIT/src/zig_clang_driver.cpp llvm-project/clang/tools/driver/cc1_main.cpp is copied to $ZIGGIT/src/zig_clang_cc1_main.cpp llvm-project/clang/tools/driver/cc1as_main.cpp is copied to $ZIGGIT/src/zig_clang_cc1as_main.cpp The following patch is applied: --- a/src/zig_clang_driver.cpp +++ b/src/zig_clang_driver.cpp @@ -206,8 +205,6 @@ void *MainAddr); extern int cc1as_main(ArrayRef<const char *> Argv, const char *Argv0, void *MainAddr); -extern int cc1gen_reproducer_main(ArrayRef<const char *> Argv, - const char *Argv0, void *MainAddr); static void insertTargetAndModeArgs(const ParsedClangName &NameParts, SmallVectorImpl<const char *> &ArgVector, @@ -330,19 +327,18 @@ if (Tool == "-cc1as") return cc1as_main(makeArrayRef(ArgV).slice(2), ArgV[0], GetExecutablePathVP); - if (Tool == "-cc1gen-reproducer") - return cc1gen_reproducer_main(makeArrayRef(ArgV).slice(2), ArgV[0], - GetExecutablePathVP); // Reject unknown tools. llvm::errs() << "error: unknown integrated tool ''" << Tool << "''. " << "Valid tools include ''-cc1'' and ''-cc1as''.\n"; return 1; } -int main(int argc_, const char **argv_) { +extern "C" int ZigClang_main(int argc_, const char **argv_); +int ZigClang_main(int argc_, const char **argv_) { noteBottomOfStack(); llvm::InitLLVM X(argc_, argv_); - SmallVector<const char *, 256> argv(argv_, argv_ + argc_); + size_t argv_offset = (strcmp(argv_[1], "-cc1") == 0 || strcmp(argv_[1], "-cc1as") == 0) ? 0 : 1; + SmallVector<const char *, 256> argv(argv_ + argv_offset, argv_ + argc_); if (llvm::sys::Process::FixupStandardFileDescriptors()) return 1; This disables some cruft, and then renames main to ZigClang_main so that it can be called like any other function. Next, in Zig''s actual main, it looks for clang as the first parameter, and calls it. So, zig clang is low-level undocumented API that Zig exposes for directly invoking Clang. But zig cc is much higher level than that. When Zig needs to compile C code, it invokes itself as a child process, taking advantage of zig clang. zig cc on the other hand, has a more difficult job: it must parse Clang''s command line options and map those to the Zig compiler''s settings, so that ultimately zig clang can be invoked as a child process. Parsing Clang Command Line Options When using zig cc, Zig acts as a proxy between the user and Clang. It does not need to understand all the parameters, but it does need to understand some of them, such as the target. This means that Zig must understand when a C command line parameter expects to "consume" the next parameter on the command line. For example, -z -target would mean to pass -target to the linker, whereas -E -target would mean that the next parameter specifies the target. Clang has a long list of command line options and so it would be foolish to try to hard-code all of them. Fortunately, LLVM has a file "options.td" which describes all of its command line parameter options in some obscure format. But fortunately again, LLVM comes with the llvm-tblgen tool that can dump it as JSON format. Zig has an update_clang_options tool which processes this JSON dump and produces a big sorted list of Clang''s command line options. Combined with a list of "known options" which correspond to Zig compiler options, this is used to make an iterator API that zig cc uses to parse command line parameters and instantiate a Zig compiler instance. Any Clang options that Zig is not aware of are forwarded to Clang directly. Some parameters are handled specially. Linking This part is pretty straightforward. Zig depends on LLD for linking rather than shelling out to the system linker, like GCC and Clang do. When you use -o with zig cc, Clang is not actually acting as a linker driver here. Zig is still the linker driver. Everybody Wins Now that I''ve spent this entire blog article comparing Zig and Clang as if they are competitors, let me make it absolutely clear that both of these are harmonious, mutually beneficial open-source projects. It''s pretty obvious how Clang and the entire LLVM project are massively beneficial to the Zig project, since Zig builds on top of them. But it works the other way, too. With Zig''s focus on cross-compiling, its test suite has been expanding rapidly to cover a large number of architectures and operating systems, leading to dozens of bugs reported upstream and patches sent, including, for example: Regression discovered in LLVM 9 release candidate Bug fixed in Wine''s NtDll Directly working with RISC-V target developers Bug fixes in LLVM''s MIPS code generation Everybody wins. This is still experimental! I have only recently landed zig cc support last week, and it is still experimental. Please do not expect it to be production quality yet. Zig''s 0.6.0 release is right around the corner, scheduled for April 13th. I will be sure to provide an update on the release notes on how stable and robust you can expect zig cc to be in the 0.6.0 release. There are some follow-up issues related to zig cc which are still open: improve zig cc flag integration using zig as a drop in replacement for msvc support compiling and linking c++ code use case: directly symlink zig binary to /usr/bin/cc As always, Contributions are most welcome. 💖 Sponsor Zig 💖 Sponsor Andrew Kelley on GitHub If you''re reading this and you already sponsor me, thank you so much! I wake up every day absolutely thrilled that I get to do this for my full time job. As Zig has been gaining popularity, demands for my time have been growing faster than funds to hire another full-time programmer. Every recurring donation helps, and if the funds keep growing then soon enough the Zig project will have two full-time programmers. That''s all folks. I hope you and your loved ones are well.'
content: extracted
html: 2020-03-24-zig-cc-a-powerful-drop-in-replacement-for-gcc-clang.html
---

[Andrew Kelley](https://andrewkelley.me/) - \`zig cc\`: a Powerful Drop-In Replacement for GCC/Clang (2020 Mar 24)

If you have heard of [Zig](https://ziglang.org/) before, you may know it as a promising new programming language which is ambitiously trying to overthrow C as the de-facto systems language. But did you know that it also can straight up compile C code?

This has been possible for a while, and you can see some [examples of this on the home page](https://ziglang.org/#Zig-is-also-a-C-compiler). What's new is that the `zig cc` sub-command is available, and it supports the same options as [Clang](https://clang.llvm.org/), which, in turn, supports the same options as [GCC](https://gcc.gnu.org/).

Now, I'm sure you're feeling pretty skeptical right about now, so let me hook you real quick before I get into the juicy details.

## Clang and GCC cannot do this:

```
andy@ark ~/tmp> cat hello.c
#include <stdio.h>

int main(int argc, char **argv) {
    fprintf(stderr, "Hello, World!\n");
    return 0;
}
andy@ark ~/tmp> clang -o hello.exe hello.c -target x86_64-windows-gnu
clang-7: warning: argument unused during compilation: '--gcc-toolchain=/nix/store/ificps9si1nvz85f9xa7gjd9h6r5lzg6-gcc-9.2.0' [-Wunused-command-line-argument]
/nix/store/7bhi29ainf5rjrk7k7wyhndyskzyhsxh-binutils-2.31.1/bin/ld: unrecognised emulation mode: i386pep
Supported emulations: elf_x86_64 elf32_x86_64 elf_i386 elf_iamcu elf_l1om elf_k1om
clang-7: error: linker command failed with exit code 1 (use -v to see invocation)
andy@ark ~/tmp> clang -o hello hello.c -target mipsel-linux-musl
In file included from hello.c:1:
In file included from /nix/store/8pp3i3hcp7bv0f8jllzqq7gcp9dbzvp9-glibc-2.27-dev/include/stdio.h:27:
In file included from /nix/store/8pp3i3hcp7bv0f8jllzqq7gcp9dbzvp9-glibc-2.27-dev/include/bits/libc-header-start.h:33:
In file included from /nix/store/8pp3i3hcp7bv0f8jllzqq7gcp9dbzvp9-glibc-2.27-dev/include/features.h:452:
/nix/store/8pp3i3hcp7bv0f8jllzqq7gcp9dbzvp9-glibc-2.27-dev/include/gnu/stubs.h:7:11: fatal error: 
      'gnu/stubs-32.h' file not found
# include <gnu/stubs-32.h>
          ^~~~~~~~~~~~~~~~
1 error generated.
andy@ark ~/tmp> clang -o hello hello.c -target aarch64-linux-gnu
In file included from hello.c:1:
In file included from /nix/store/8pp3i3hcp7bv0f8jllzqq7gcp9dbzvp9-glibc-2.27-dev/include/stdio.h:27:
In file included from /nix/store/8pp3i3hcp7bv0f8jllzqq7gcp9dbzvp9-glibc-2.27-dev/include/bits/libc-header-start.h:33:
In file included from /nix/store/8pp3i3hcp7bv0f8jllzqq7gcp9dbzvp9-glibc-2.27-dev/include/features.h:452:
/nix/store/8pp3i3hcp7bv0f8jllzqq7gcp9dbzvp9-glibc-2.27-dev/include/gnu/stubs.h:7:11: fatal error: 
      'gnu/stubs-32.h' file not found
# include <gnu/stubs-32.h>
          ^~~~~~~~~~~~~~~~
1 error generated.
```

## \`zig cc\` can:

```
andy@ark ~/tmp> zig cc -o hello.exe hello.c -target x86_64-windows-gnu
andy@ark ~/tmp> wine64 hello.exe
Hello, World!
andy@ark ~/tmp> zig cc -o hello hello.c -target mipsel-linux-musl
andy@ark ~/tmp> qemu-mipsel ./hello
Hello, World!
andy@ark ~/tmp> zig cc -o hello hello.c -target aarch64-linux-gnu
andy@ark ~/tmp> qemu-aarch64 -L ~/Downloads/glibc/multi-2.31/install/glibcs/aarch64-linux-gnu ./hello
Hello, World!
```

## Features of \`zig cc\`

`zig cc` is *not the main purpose of the Zig project*. It merely exposes the already-existing capabilities of the Zig compiler via a small frontend layer that parses C compiler options.

### Install simply by unzipping a tarball

Zig is an open source project, and of course can be [built and installed from source the usual way](https://github.com/ziglang/zig/#building-from-source). However, the Zig project also has tarballs available on [the download page](https://ziglang.org/download/). You can download a 45 MiB tarball, unpack it, and you're done. You can even have multiple versions at the same time, no problem.

Here, rather than downloading the x86\_64-linux version, which matches the computer I am currently using, I'll download the Windows version and run it in [Wine](https://www.winehq.org/) to show how simple installation is:

```
andy@ark ~/tmp> wget --quiet https://ziglang.org/builds/zig-windows-x86_64-0.5.0+13d04f996.zip
andy@ark ~/tmp> unzip -q zig-windows-x86_64-0.5.0+13d04f996.zip 
andy@ark ~/tmp> wine64 ./zig-windows-x86_64-0.5.0+13d04f996/zig.exe cc -o hello hello.c -target x86_64-linux
andy@ark ~/tmp> ./hello
Hello, World!
```

Take a moment to appreciate what just happened here - I downloaded a Windows build of Zig, ran it in Wine, using it to cross compile for Linux, and then ran the binary natively. Computers are fun!

Compare this to [downloading Clang](https://github.com/llvm/llvm-project/releases/tag/llvmorg-9.0.1), which has 380 MiB Linux-distribution-specific tarballs. Zig's Linux tarballs are fully statically linked, and therefore work correctly on all Linux distributions. The size difference here comes because the Clang tarball ships with more utilities than a C compiler, as well as pre-compiled static libraries for both LLVM and Clang. Zig does not ship with any pre-compiled libraries; instead it ships with source code, and builds what it needs on-the-fly.

### Caching System

The Zig compiler uses a sophisticated caching system to avoid needlessly rebuilding artifacts. I carefully designed this caching system to make optimal use of the file system while maintaining correct semantics - which is [trickier than you might think](https://apenwarr.ca/log/20181113)!

The caching system uses a combination of hashing inputs and checking the fstat values of file paths, while being mindful of mtime granularity. This makes it avoid needlessly hashing files, while at the same time detecting when a modified file has the same contents. It always has correct behavior, whether the file system has nanosecond mtime granularity, second granularity, always sets mtime to zero, or anything in between.

You can find a [detailed description of the caching system in the 0.4.0 release notes](https://ziglang.org/download/0.4.0/release-notes.html#Build-Artifact-Caching).

`zig cc` makes this caching system available when compiling C code. For simple enough projects, this obviates the need for a Makefile or other build system.

```
andy@ark ~/tmp> cat foo.c
#include <stdio.h>

#include "another_file.c"

int main(int argc, char **argv) {
#include "printf_many_times.c"
}
andy@ark ~/tmp> cat another_file.c 
void another(void) {}
andy@ark ~/tmp> time zig cc -c foo.c
0.12
andy@ark ~/tmp> time zig cc -c foo.c
0.01
andy@ark ~/tmp> touch another_file.c 
andy@ark ~/tmp> time zig cc -c foo.c
0.01
andy@ark ~/tmp> echo "/* add a comment */" >>another_file.c
andy@ark ~/tmp> time zig cc -c foo.c
0.12
andy@ark ~/tmp> time zig cc -c foo.c
0.01
```

Here you can see the caching system is smart enough to find dependencies that are included with the preprocessor, and smart enough to avoid a full rebuild when the mtime of another\_file.c was updated.

One last thing before I move on. I want to point out that this caching system is not some fluffy bloated feature - rather it is an absolutely critical component to making cross-compiling work in a usable manner. As we'll see below, other compilers ship with pre-compiled, target-specific binaries, while Zig ships with *source code only* and cross-compiles on-the-fly, caching the result.

### Cross Compiling

I have carefully designed Zig since the very beginning to treat cross compilation as a first class use case. Now that the `zig cc` frontend is available, it brings these capabilities to C code.

I showed you above cross-compiling some simple "Hello, World!" programs. But now let's try a real-world C project.

Let's try [LuaJIT](https://luajit.org/)!

```
[~/Downloads]$ git clone https://github.com/LuaJIT/LuaJIT
[~/Downloads]$ cd LuaJIT
[~/Downloads/LuaJIT]$ ls
COPYRIGHT  doc  dynasm  etc  Makefile  README  src
```

OK so it uses standard Makefiles. Here we go, first let's make sure it works natively with `zig cc`.

```
[~/Downloads/LuaJIT]$ export CC="zig cc"
[~/Downloads/LuaJIT]$ make CC="$CC"
==== Building LuaJIT 2.1.0-beta3 ====
make -C src
make[1]: Entering directory '/home/andy/Downloads/LuaJIT/src'
HOSTCC    host/minilua.o
HOSTLINK  host/minilua
DYNASM    host/buildvm_arch.h
HOSTCC    host/buildvm.o
HOSTCC    host/buildvm_asm.o
HOSTCC    host/buildvm_peobj.o
HOSTCC    host/buildvm_lib.o
HOSTCC    host/buildvm_fold.o
HOSTLINK  host/buildvm
BUILDVM   lj_vm.S
ASM       lj_vm.o
CC        lj_gc.o
BUILDVM   lj_ffdef.h
CC        lj_err.o
CC        lj_char.o
BUILDVM   lj_bcdef.h
CC        lj_bc.o
CC        lj_obj.o
CC        lj_buf.o
CC        lj_str.o
CC        lj_tab.o
CC        lj_func.o
CC        lj_udata.o
CC        lj_meta.o
CC        lj_debug.o
CC        lj_state.o
CC        lj_dispatch.o
CC        lj_vmevent.o
CC        lj_vmmath.o
CC        lj_strscan.o
CC        lj_strfmt.o
CC        lj_strfmt_num.o
CC        lj_api.o
CC        lj_profile.o
CC        lj_lex.o
CC        lj_parse.o
CC        lj_bcread.o
CC        lj_bcwrite.o
CC        lj_load.o
CC        lj_ir.o
CC        lj_opt_mem.o
BUILDVM   lj_folddef.h
CC        lj_opt_fold.o
CC        lj_opt_narrow.o
CC        lj_opt_dce.o
CC        lj_opt_loop.o
CC        lj_opt_split.o
CC        lj_opt_sink.o
CC        lj_mcode.o
CC        lj_snap.o
CC        lj_record.o
CC        lj_crecord.o
BUILDVM   lj_recdef.h
CC        lj_ffrecord.o
CC        lj_asm.o
CC        lj_trace.o
CC        lj_gdbjit.o
CC        lj_ctype.o
CC        lj_cdata.o
CC        lj_cconv.o
CC        lj_ccall.o
CC        lj_ccallback.o
CC        lj_carith.o
CC        lj_clib.o
CC        lj_cparse.o
CC        lj_lib.o
CC        lj_alloc.o
CC        lib_aux.o
BUILDVM   lj_libdef.h
CC        lib_base.o
CC        lib_math.o
CC        lib_bit.o
CC        lib_string.o
CC        lib_table.o
CC        lib_io.o
CC        lib_os.o
CC        lib_package.o
CC        lib_debug.o
CC        lib_jit.o
CC        lib_ffi.o
CC        lib_init.o
AR        libluajit.a
CC        luajit.o
BUILDVM   jit/vmdef.lua
DYNLINK   libluajit.so
LINK      luajit
warning: unsupported linker arg: -E
OK        Successfully built LuaJIT
make[1]: Leaving directory '/home/andy/Downloads/LuaJIT/src'
==== Successfully built LuaJIT 2.1.0-beta3 ====

[~/Downloads/LuaJIT]$ ls
COPYRIGHT  doc  dynasm  etc  Makefile  README  src

[~/Downloads/LuaJIT]$ ./src/
host/         jit/          libluajit.so  luajit        zig-cache/    

[~/Downloads/LuaJIT]$ ./src/luajit 
LuaJIT 2.1.0-beta3 -- Copyright (C) 2005-2020 Mike Pall. http://luajit.org/
JIT: ON SSE2 SSE3 SSE4.1 BMI2 fold cse dce fwd dse narrow loop abc sink fuse
> print(3 + 4)
7
> 
```

OK so that worked. Now for the real test - can we make it cross compile?

```
[~/Downloads/LuaJIT]$ git clean -xfdq
[~/Downloads/LuaJIT]$ export CC="zig cc -target aarch64-linux-gnu"
[~/Downloads/LuaJIT]$ export HOST_CC="zig cc"
[~/Downloads/LuaJIT]$ make CC="$CC" HOST_CC="$HOST_CC" TARGET_STRIP="echo"
==== Building LuaJIT 2.1.0-beta3 ====
make -C src
make[1]: Entering directory '/home/andy/Downloads/LuaJIT/src'
HOSTCC    host/minilua.o
HOSTLINK  host/minilua
DYNASM    host/buildvm_arch.h
HOSTCC    host/buildvm.o
HOSTCC    host/buildvm_asm.o
HOSTCC    host/buildvm_peobj.o
HOSTCC    host/buildvm_lib.o
HOSTCC    host/buildvm_fold.o
HOSTLINK  host/buildvm
BUILDVM   lj_vm.S
ASM       lj_vm.o
CC        lj_gc.o
BUILDVM   lj_ffdef.h
CC        lj_err.o
CC        lj_char.o
BUILDVM   lj_bcdef.h
CC        lj_bc.o
CC        lj_obj.o
CC        lj_buf.o
CC        lj_str.o
CC        lj_tab.o
CC        lj_func.o
CC        lj_udata.o
CC        lj_meta.o
CC        lj_debug.o
CC        lj_state.o
CC        lj_dispatch.o
CC        lj_vmevent.o
CC        lj_vmmath.o
CC        lj_strscan.o
CC        lj_strfmt.o
CC        lj_strfmt_num.o
CC        lj_api.o
CC        lj_profile.o
CC        lj_lex.o
CC        lj_parse.o
CC        lj_bcread.o
CC        lj_bcwrite.o
CC        lj_load.o
CC        lj_ir.o
CC        lj_opt_mem.o
BUILDVM   lj_folddef.h
CC        lj_opt_fold.o
CC        lj_opt_narrow.o
CC        lj_opt_dce.o
CC        lj_opt_loop.o
CC        lj_opt_split.o
CC        lj_opt_sink.o
CC        lj_mcode.o
CC        lj_snap.o
CC        lj_record.o
CC        lj_crecord.o
BUILDVM   lj_recdef.h
CC        lj_ffrecord.o
CC        lj_asm.o
CC        lj_trace.o
CC        lj_gdbjit.o
CC        lj_ctype.o
CC        lj_cdata.o
CC        lj_cconv.o
CC        lj_ccall.o
CC        lj_ccallback.o
CC        lj_carith.o
CC        lj_clib.o
CC        lj_cparse.o
CC        lj_lib.o
CC        lj_alloc.o
CC        lib_aux.o
BUILDVM   lj_libdef.h
CC        lib_base.o
CC        lib_math.o
CC        lib_bit.o
CC        lib_string.o
CC        lib_table.o
CC        lib_io.o
CC        lib_os.o
CC        lib_package.o
CC        lib_debug.o
CC        lib_jit.o
CC        lib_ffi.o
CC        lib_init.o
AR        libluajit.a
CC        luajit.o
BUILDVM   jit/vmdef.lua
DYNLINK   libluajit.so
libluajit.so
LINK      luajit
warning: unsupported linker arg: -E
luajit
OK        Successfully built LuaJIT
make[1]: Leaving directory '/home/andy/Downloads/LuaJIT/src'
==== Successfully built LuaJIT 2.1.0-beta3 ====

[~/Downloads/LuaJIT]$ file ./src/luajit 
./src/luajit: ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-aarch64.so.1, for GNU/Linux 2.0.0, with debug_info, not stripped
```

It worked! Will it run in [QEMU](https://www.qemu.org/) though?

```
[~/Downloads/LuaJIT]$ qemu-aarch64 -L ~/Downloads/glibc/multi-2.31/install/glibcs/aarch64-linux-gnu ./src/luajit
LuaJIT 2.1.0-beta3 -- Copyright (C) 2005-2020 Mike Pall. http://luajit.org/
JIT: ON fold cse dce fwd dse narrow loop abc sink fuse
> print(4 + 3)
7
> 
```

Amazing. QEMU never fails to impress me.

Before we move on, I want to show one more thing. You can see above, in order to run the foreign-architecture binary, I had to pass `-L ~/Downloads/glibc/multi-2.31/install/glibcs/aarch64-linux-gnu`. This is due to the binary being dynamically linked. You can confirm this with the output from `file` above where it says: `dynamically linked, interpreter /lib/ld-linux-aarch64.so.1`

Often, when cross-compiling, it is useful to make a *static* binary. In the case of Linux, for example, this will make the resulting binary able to run on any Linux distribution, rather than only ones with a hard-coded glibc dynamic linker path of `/lib/ld-linux-aarch64.so.1`.

We can accomplish this by targeting musl rather than glibc:

```
[~/Downloads/LuaJIT]$ git clean -qxfd
[~/Downloads/LuaJIT]$ export CC="zig cc -target aarch64-linux-musl"
[~/Downloads/LuaJIT]$ make CC="$CC" CXX="$CXX" HOST_CC="$HOST_CC" TARGET_STRIP="echo"
==== Building LuaJIT 2.1.0-beta3 ====
(same output)
==== Successfully built LuaJIT 2.1.0-beta3 ====
[~/Downloads/LuaJIT]$ file src/luajit
src/luajit: ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), statically linked, not stripped
[~/Downloads/LuaJIT]$ qemu-aarch64 ./src/luajit
LuaJIT 2.1.0-beta3 -- Copyright (C) 2005-2020 Mike Pall. http://luajit.org/
JIT: ON fold cse dce fwd dse narrow loop abc sink fuse
> print(11 + 22)
33
```

Here you can see the `file` command reported *statically linked*, and in the qemu command, the `-L` parameter was not needed.

## Use Cases of \`zig cc\`

Alright, so I've given you a taste of what `zig cc` can do, but now I will list explicitly what I consider to be the use cases:

### Experimentation

Sometimes you just want a tool that you can use to try out different things. It can quickly answer questions such as "What assembly does this code generate on MIPS vs ARM?". The widely popular [Compiler Explorer](https://godbolt.org/) serves this purpose.

`zig cc` provides a lightweight tool which can also answer questions such as, "What happens if I swap out glibc for [musl](https://musl.libc.org/)?" and "How big is this executable when cross-compiled for Windows?". [Here's me using Zig to quickly find out what the maximum UDP packet size is on Linux](https://twitter.com/andy_kelley/status/1242183564512366595).

Since Zig is so easy to install - and it actually works everywhere without patches, even Linux distributions such as [NixOS](https://nixos.org/) - it can often be a more convenient tool for running quick C test programs on your computer.

At the time of this writing, LLVM 10 was just released two hours ago. It will take days or weeks for it to become available in various system package managers. But you can already [download a master branch build of Zig](https://ziglang.org/download/) and play with the new features of Clang/LLVM 10. For example, improved RISC-V support!

```
andy@ark ~/tmp> zig cc -o hello hello.c -target riscv64-linux-musl
andy@ark ~/tmp> qemu-riscv64 ./hello
Hello, World!
```

### Bundling a C compiler as part of a larger project

With Zig tarballs weighing in at under 45 MiB, zero system dependencies, no configuration, and MIT license, it makes for an ideal candidate when you need to bundle a C compiler along with another project.

For example, maybe you have [a programming language that compiles to C](https://www.acton-lang.org/). Zig is an obvious choice for what C compiler to ship with your language.

Or maybe you want to make a batteries-included [IDE](https://en.wikipedia.org/wiki/Integrated_development_environment) that ships with a compiler.

### Lightweight alternative to a cross compilation environment

If you're trying to build something with a large dependency tree, you'll probably want to use a full cross compilation environment, such as [mxe.cc](https://mxe.cc/) or [musl.cc](http://musl.cc/).

But if you don't need such a sledgehammer, `zig cc` could be a useful alternative, especially if your goal is to compile for N different targets. Consider that musl.cc lists different tarballs for each architecture, each weighing in at roughly 85 MiB. Meanwhile Zig weighs in at 45 MiB and it supports all those architectures, plus glibc and Windows.

### An alternative to installing MSVC on Windows

You could spend days - literally! - waiting for Microsoft Visual Studio to install, or you could install Zig and [VS Code](https://code.visualstudio.com/) in a matter of minutes.

## Under the Hood

If `zig cc` is built on top of Clang, why doesn't Clang just do this? What exactly is Zig doing on top of Clang to make this work?

The answer is, *a lot*, actually. I'll go over how it works here.

### compiler-rt

compiler-rt is a library that provides "polyfill" implementations of language-supported features when the target does not have machine code instructions for it. For example, compiler-rt has the function `__muldi3` to perform signed 64-bit integer multiplication on architectures that do not have a 64-bit wide integer multiplication instruction.

In the GNU world, compiler-rt is named **libgcc**.

Most C compilers ship with this library pre-built for the target. For example, on an Ubuntu (Bionic) system, with the `build-essential` package installed, you can find this at `/lib/x86_64-linux-gnu/libgcc_s.so.1`.

If you download [clang+llvm-9.0.1-x86\_64-linux-gnu-ubuntu-16.04.tar.xz](https://github.com/llvm/llvm-project/releases/download/llvmorg-9.0.1/clang+llvm-9.0.1-x86_64-linux-gnu-ubuntu-16.04.tar.xz) and take a look around, clang actually does not even ship with compiler-rt. Instead, it relies on the system libgcc noted above. This is one reason that this tarball is Ubuntu-specific and does not work on other Linux distributions, [FreeBSD's Linuxulator](https://www.leidinger.net/blog/2010/09/28/the-freebsd-linuxulator-explained-for-users/), or [WSL](https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux), which have system files in different locations.

Zig's strategy with compiler-rt is that we have [our own implementation of this library](https://github.com/ziglang/zig/blob/0.5.0/lib/std/special/compiler_rt.zig), written in Zig. Most of it is ported from [LLVM's compiler-rt project](https://github.com/llvm/llvm-project/tree/llvmorg-10.0.0-rc6/compiler-rt), but we also have some of our own improvements on top of this.

Anyway, rather than depending on system compiler-rt being installed, or shipping a pre-compiled library, Zig ships its compiler-rt *in source form*, and lazily builds compiler-rt for the compilation target, and then caches the result using [the caching system discussed above](https://andrewkelley.me/post/zig-cc-powerful-drop-in-replacement-gcc-clang.html#caching-system).

Zig's compiler-rt is [not yet complete](https://github.com/ziglang/zig/issues/1290). However, completing it is a prerequisite for releasing Zig version 1.0.0.

### libc

When C code calls `printf`, `printf` has to be implemented *somewhere*, and that somewhere is libc.

Some operating systems, such as [FreeBSD](https://www.freebsd.org/) and macOS, have a designated system libc, and it is the kernel syscall interface. On others, such as Windows and Linux, libc is optional, and therefore there are multiple options of which libc to use, if any.

As of the time of this writing, Zig can provide libcs for the following targets:

```
andy@ark ~> zig targets | jq .libc
[
  "aarch64_be-linux-gnu",
  "aarch64_be-linux-musl",
  "aarch64_be-windows-gnu",
  "aarch64-linux-gnu",
  "aarch64-linux-musl",
  "aarch64-windows-gnu",
  "armeb-linux-gnueabi",
  "armeb-linux-gnueabihf",
  "armeb-linux-musleabi",
  "armeb-linux-musleabihf",
  "armeb-windows-gnu",
  "arm-linux-gnueabi",
  "arm-linux-gnueabihf",
  "arm-linux-musleabi",
  "arm-linux-musleabihf",
  "arm-windows-gnu",
  "i386-linux-gnu",
  "i386-linux-musl",
  "i386-windows-gnu",
  "mips64el-linux-gnuabi64",
  "mips64el-linux-gnuabin32",
  "mips64el-linux-musl",
  "mips64-linux-gnuabi64",
  "mips64-linux-gnuabin32",
  "mips64-linux-musl",
  "mipsel-linux-gnu",
  "mipsel-linux-musl",
  "mips-linux-gnu",
  "mips-linux-musl",
  "powerpc64le-linux-gnu",
  "powerpc64le-linux-musl",
  "powerpc64-linux-gnu",
  "powerpc64-linux-musl",
  "powerpc-linux-gnu",
  "powerpc-linux-musl",
  "riscv64-linux-gnu",
  "riscv64-linux-musl",
  "s390x-linux-gnu",
  "s390x-linux-musl",
  "sparc-linux-gnu",
  "sparcv9-linux-gnu",
  "wasm32-freestanding-musl",
  "x86_64-linux-gnu",
  "x86_64-linux-gnux32",
  "x86_64-linux-musl",
  "x86_64-windows-gnu"
]
```

In order to provide libc on these targets, Zig ships with a subset of the source files for these projects:

- musl v1.2.0
- [mingw-w64](https://mingw-w64.org/) v7.0.0
- glibc 2.31

For each libc, there is a [process for upgrading to a new release](https://github.com/ziglang/zig/wiki/Updating-libc). This process is a sort of pre-processing step. We still end up with source files, but we de-duplicate non-multi-arch source files into multi-arch source files.

#### glibc

glibc is the most involved. The first step is building glibc for every target that it supports, which takes upwards of 24 hours and 74 GiB of disk space.

From here, the [process\_headers tool](https://github.com/ziglang/zig/blob/dc44fe053c609f389e375f6857f96b6bb3794897/tools/process_headers.zig) inspects all the header files from all the targets, and identifies which files are the same across all targets, and which header files are target-specific. They are then sorted into the corresponding directories in Zig's source tree, in:

- lib/libc/include/generic-glibc/
- lib/libc/include/$ARCH-linux-$ABI/ (there are multiple of these directories)

Additionally, Linux header files are not included in glibc, and so the same process is applied to Linux header files, with the directories:

- lib/libc/include/any-linux-any/
- lib/libc/include/$ARCH-linux-any/

That takes care of the header files, but now we have the problem of dynamic linking against glibc, without touching any system files.

For this, we have the [update\_glibc tool](https://github.com/ziglang/zig/blob/dc44fe053c609f389e375f6857f96b6bb3794897/tools/update_glibc.zig). Given the path to the glibc source directory, it finds all the `.abilist` text files and uses them to produce 3 simple but crucial files:

- [vers.txt](https://github.com/ziglang/zig/blob/master/lib/libc/glibc/vers.txt) - the list of all glibc versions.
- [fns.txt](https://github.com/ziglang/zig/blob/master/lib/libc/glibc/fns.txt) - the list of all symbols that glibc provides, followed by the library it appears in (for example libm, libpthread, libc, librt).
- [abi.txt](https://github.com/ziglang/zig/blob/master/lib/libc/glibc/abi.txt) - for each target, for each function, tells which versions of glibc, if any, it appears in.

Together, these files amount to only 192 KB (27 KB gzipped), and they allow Zig to target any version of glibc.

Yes, I did not make a typo there. Zig can target any of the 42 versions of glibc for any of the architectures listed above. I'll show you:

```
andy@ark ~/tmp> cat rand.zig 
const std = @import("std");

pub fn main() anyerror!void {
    var buf: [10]u8 = undefined;
    _ = std.c.getrandom(&buf, buf.len, 0);
    std.debug.warn("random bytes: {x}\n", .{buf});
}
andy@ark ~/tmp> zig build-exe rand.zig -lc -target native-native-gnu.2.25
andy@ark ~/tmp> ./rand
random bytes: e2059382afb599ea6d29
andy@ark ~/tmp> zig build-exe rand.zig -lc -target native-native-gnu.2.24
lld: error: undefined symbol: getrandom
>>> referenced by rand.zig:5 (/home/andy/tmp/rand.zig:5)
>>>               ./rand.o:(main.0)
```

Sure enough, if you look at the [man page for getrandom](http://man7.org/linux/man-pages/man2/getrandom.2.html), it says:

> Support was added to glibc in version 2.25.

When no explicit glibc version is requested, and the target OS is the native (host) OS, Zig detects the native glibc version by inspecting the Zig executable's own dynamically linked libraries, looking for glibc, and checking the version. It turns out you can look for `libc.so.6` and then `readlink` on that, and it will look something like `libc-2.27.so`. When this strategy does not work, Zig looks at `/usr/bin/env`, looking for the same thing. Since this file path is hard-coded into countless shebang lines, it's a pretty safe bet to find out the dynamic linker path and glibc version (if any) of the native system!

`zig cc` currently does not provide a way to choose a specific glibc version (because C compilers do not provide a way), and so Zig chooses the native version for compiling natively, and the default (2.17) for cross-compiling. However, I'm sure this problem can be solved, even when using `zig cc`. For example, maybe it could support an environment variable, or simply introduce an extra command line option that does not conflict with any Clang options.

When you request a certain version of glibc, Zig uses those text files noted above to create dummy `.so` files to link against, which contain exactly the correct set of symbols (with appropriate name mangling) based on the requested version. The symbols will be resolved at runtime, by the dynamic linker on the target platform.

In this way, most of libc in the glibc case resides on the target file system. But not all of it! There are still the "C runtime start files":

- Scrt1.o
- crti.o
- crtn.o

These are statically compiled into every binary that dynamically links glibc, and their [ABI](https://en.wikipedia.org/wiki/Application_binary_interface) is therefore Very Very Stable.

And so, Zig bundles a small subset of glibc's source files needed to build these object files from source for every target. The total size of this comes out to 1.4 MiB (252 KB gzipped). I do think there is some room for improvement here, but I digress.

There are a couple of patches to this small subset of glibc source files, which simplify them to avoid including too many .h files, since the end result that we need is some bare bones object files, and not all of glibc.

And finally, we certainly do not ship the build system of glibc with Zig! I manually inspected, audited, and analyzed glibc's build system, and then by hand wrote code in the Zig compiler which hooks into Zig's [caching system](https://andrewkelley.me/post/zig-cc-powerful-drop-in-replacement-gcc-clang.html#caching-system) and performs a minimal build of only these start files, as needed.

#### musl

The process for preparing musl to ship with Zig is much simpler by comparison.

It still involves building musl for every target architecture that it supports, but in this case only the `install-headers` target has to be run, and it takes less than a minute, even to do it for all targets.

The same [process\_headers tool](https://github.com/ziglang/zig/blob/dc44fe053c609f389e375f6857f96b6bb3794897/tools/process_headers.zig) tool used for glibc headers is used on the musl headers:

- lib/libc/include/generic-musl/
- lib/libc/include/$ARCH-linux-$ABI/ (there are multiple of these directories)

Unlike glibc, musl supports building statically. Zig currently assumes a static libc when musl is chosen, and does not support dynamically linking against musl, although that could potentially be added in the future.

And so for musl, zig actually bundles most - but still not all - of musl's source files. Everything in `arch`, `crt`, `compat`, `src`, and `include` gets copied in.

Again much like glibc, I carefully studied musl's build system, and then hand-coded logic in the Zig compiler to build these source files. In musl's case it is simpler - just a bit of logic having to do with the file extension, and whether to override files with an architecture-specific file. The only file that needs to be patched (by hand) is `version.h`, which is normally generated during the configure phase in musl's build system.

I really appreciate Rich Felker's efforts to make musl simple to utilize in this way, and he has been incredibly helpful in the `#musl` IRC channel when I ask questions. [I proudly sponsor Rich Felker for $150/month](https://andrewkelley.me/post/why-donating-to-musl-libc-project.html).

#### mingw-w64

mingw-w64 was an absolute joy to support in Zig. The beautiful thing about this project is that they have already been transitioning into having one set of header files that applies to all architectures (using `#ifdefs` only where needed). One set of header files is sufficient to support all four architectures: arm, aarch64, x86, and x86\_64.

So for updating headers, all we have to do is build mingw-w64, then:

```
mv $INSTALLPREFIX/include $ZIGSRC/lib/libc/include/any-windows-any
```

After doing this for all 3 libcs, the libc/include directory looks like this:

```
aarch64_be-linux-any   i386-linux-musl           powerpc-linux-any
aarch64_be-linux-gnu   mips64el-linux-any        powerpc-linux-gnu
aarch64-linux-any      mips64el-linux-gnuabi64   powerpc-linux-musl
aarch64-linux-gnu      mips64el-linux-gnuabin32  riscv32-linux-any
aarch64-linux-musl     mips64-linux-any          riscv64-linux-any
any-linux-any          mips64-linux-gnuabi64     riscv64-linux-gnu
any-windows-any        mips64-linux-gnuabin32    riscv64-linux-musl
armeb-linux-any        mips64-linux-musl         s390x-linux-any
armeb-linux-gnueabi    mipsel-linux-any          s390x-linux-gnu
armeb-linux-gnueabihf  mipsel-linux-gnu          s390x-linux-musl
arm-linux-any          mips-linux-any            sparc-linux-gnu
arm-linux-gnueabi      mips-linux-gnu            sparcv9-linux-gnu
arm-linux-gnueabihf    mips-linux-musl           x86_64-linux-any
arm-linux-musl         powerpc64le-linux-any     x86_64-linux-gnu
generic-glibc          powerpc64le-linux-gnu     x86_64-linux-gnux32
generic-musl           powerpc64-linux-any       x86_64-linux-musl
i386-linux-any         powerpc64-linux-gnu
i386-linux-gnu         powerpc64-linux-musl
```

When Zig generates a C command line to send to clang, it puts the appropriate include paths using `-I` depending on the target. For example, if the target is `aarch64-linux-musl`, then the following command line parameters are appended:

- `-I$LIB/libc/include/aarch64-linux-musl`
- `-I$LIB/libc/include/aarch64-linux-any`
- `-I$LIB/libc/include/generic-musl`

Anyway back to mingw-w64.

Again, Zig includes a subset of source files from mingw-w64 with a few patches applied to make things compile successfully.

The Zig compiler code that builds mingw-w64 from source files emulates only the parts of the build system that are needed for this subset. This includes preprocessing `.def.in` files to get `.def` files, and then in-turn using LLD to generate `.lib` files from the `.def` files, which allows Zig to provide `.lib` files for any Windows DLL, such as kernel32.dll or even opengl32.dll.

### Invoking Clang Without a System Dependency

Since Zig already links against Clang libraries for the [translate-c feature](https://ziglang.org/#Integration-with-C-libraries-without-FFIbindings), it was not much more cost to expose the `main()` entry point from Zig. So that's exactly what we do:

- `llvm-project/clang/tools/driver/driver.cpp` is copied to `$ZIGGIT/src/zig_clang_driver.cpp`
- `llvm-project/clang/tools/driver/cc1_main.cpp` is copied to `$ZIGGIT/src/zig_clang_cc1_main.cpp`
- `llvm-project/clang/tools/driver/cc1as_main.cpp` is copied to `$ZIGGIT/src/zig_clang_cc1as_main.cpp`

The following patch is applied:

```
--- a/src/zig_clang_driver.cpp
+++ b/src/zig_clang_driver.cpp
@@ -206,8 +205,6 @@
                     void *MainAddr);
 extern int cc1as_main(ArrayRef<const char *> Argv, const char *Argv0,
                       void *MainAddr);
-extern int cc1gen_reproducer_main(ArrayRef<const char *> Argv,
-                                  const char *Argv0, void *MainAddr);
 
 static void insertTargetAndModeArgs(const ParsedClangName &NameParts,
                                     SmallVectorImpl<const char *> &ArgVector,
@@ -330,19 +327,18 @@
   if (Tool == "-cc1as")
     return cc1as_main(makeArrayRef(ArgV).slice(2), ArgV[0],
                       GetExecutablePathVP);
-  if (Tool == "-cc1gen-reproducer")
-    return cc1gen_reproducer_main(makeArrayRef(ArgV).slice(2), ArgV[0],
-                                  GetExecutablePathVP);
   // Reject unknown tools.
   llvm::errs() << "error: unknown integrated tool '" << Tool << "'. "
                << "Valid tools include '-cc1' and '-cc1as'.\n";
   return 1;
 }
 
-int main(int argc_, const char **argv_) {
+extern "C" int ZigClang_main(int argc_, const char **argv_);
+int ZigClang_main(int argc_, const char **argv_) {
   noteBottomOfStack();
   llvm::InitLLVM X(argc_, argv_);
-  SmallVector<const char *, 256> argv(argv_, argv_ + argc_);
+  size_t argv_offset = (strcmp(argv_[1], "-cc1") == 0 || strcmp(argv_[1], "-cc1as") == 0) ? 0 : 1;
+  SmallVector<const char *, 256> argv(argv_ + argv_offset, argv_ + argc_);
 
   if (llvm::sys::Process::FixupStandardFileDescriptors())
     return 1;
```

This disables some cruft, and then renames `main` to `ZigClang_main` so that it can be called like any other function. Next, in Zig's actual `main`, it looks for `clang` as the first parameter, and calls it.

So, `zig clang` is low-level undocumented API that Zig exposes for directly invoking Clang. But `zig cc` is much higher level than that. When Zig needs to compile C code, it invokes itself as a child process, taking advantage of `zig clang`. `zig cc` on the other hand, has a more difficult job: it must parse Clang's command line options and map those to the Zig compiler's settings, so that ultimately `zig clang` can be invoked as a child process.

### Parsing Clang Command Line Options

When using `zig cc`, Zig acts as a proxy between the user and Clang. It does not need to understand all the parameters, but it does need to understand some of them, such as the target. This means that Zig must understand when a C command line parameter expects to "consume" the next parameter on the command line.

For example, `-z -target` would mean to pass `-target` to the linker, whereas `-E -target` would mean that the next parameter specifies the target.

Clang has a [long list of command line options](https://clang.llvm.org/docs/ClangCommandLineReference.html) and so it would be foolish to try to hard-code all of them.

Fortunately, LLVM has a file "options.td" which describes all of its command line parameter options in some obscure format. But fortunately again, LLVM comes with the `llvm-tblgen` tool that can dump it as JSON format.

Zig has an [update\_clang\_options tool](https://github.com/ziglang/zig/blob/dc44fe053c609f389e375f6857f96b6bb3794897/tools/update_clang_options.zig) which processes this JSON dump and produces a [big sorted list of Clang's command line options](https://github.com/ziglang/zig/blob/dc44fe053c609f389e375f6857f96b6bb3794897/src-self-hosted/clang_options_data.zig).

Combined with a list of "known options" which correspond to Zig compiler options, this is used to make an iterator API that `zig cc` uses to parse command line parameters and instantiate a Zig compiler instance. Any Clang options that Zig is not aware of are forwarded to Clang directly. Some parameters are handled specially.

### Linking

This part is pretty straightforward. Zig depends on LLD for linking rather than shelling out to the system linker, like GCC and Clang do.

When you use `-o` with `zig cc`, Clang is not actually acting as a linker driver here. Zig is still the linker driver.

## Everybody Wins

Now that I've spent this entire blog article comparing Zig and Clang as if they are competitors, let me make it absolutely clear that both of these are harmonious, mutually beneficial open-source projects. It's pretty obvious how Clang and the entire LLVM project are massively beneficial to the Zig project, since Zig builds on top of them.

But it works the other way, too.

With Zig's focus on cross-compiling, its test suite has been expanding rapidly to cover a large number of architectures and operating systems, leading to [dozens of bugs reported upstream and patches sent](https://github.com/ziglang/zig/issues?q=is%3Aissue+label%3Aupstream+is%3Aclosed), including, for example:

- [Regression discovered in LLVM 9 release candidate](https://bugs.llvm.org/show_bug.cgi?id=43268)
- [Bug fixed in Wine's NtDll](https://bugs.winehq.org/show_bug.cgi?id=47979)
- [Directly working with RISC-V target developers](https://github.com/ziglang/zig/issues/3338#issuecomment-536771508)
- [Bug fixes in LLVM's MIPS code generation](https://bugs.llvm.org/show_bug.cgi?id=43768#c3)

Everybody wins.

## This is still experimental!

I have only recently landed `zig cc` support last week, and it is still experimental. Please do not expect it to be production quality yet.

Zig's 0.6.0 release is right around the corner, scheduled for April 13th. I will be sure to provide an update on the release notes on how stable and robust you can expect `zig cc` to be in the 0.6.0 release.

There are some follow-up issues related to `zig cc` which are still open:

- [improve zig cc flag integration](https://github.com/ziglang/zig/issues/4784)
- [using zig as a drop in replacement for msvc](https://github.com/ziglang/zig/issues/4785)
- [support compiling and linking c++ code](https://github.com/ziglang/zig/issues/4786)
- [use case: directly symlink zig binary to /usr/bin/cc](https://github.com/ziglang/zig/issues/4787)

As always, [Contributions are most welcome](https://github.com/ziglang/zig/blob/master/CONTRIBUTING.md).

## 💖 Sponsor Zig 💖

[Sponsor Andrew Kelley on GitHub](https://github.com/sponsors/andrewrk)

If you're reading this and you already sponsor me, thank you so much! I wake up every day absolutely thrilled that I get to do this for my full time job.

As Zig has been gaining popularity, demands for my time have been growing faster than funds to hire another full-time programmer. Every recurring donation helps, and if the funds keep growing then soon enough the Zig project will have two full-time programmers.

That's all folks. I hope you and your loved ones are well.

Thanks for reading my blog post.
