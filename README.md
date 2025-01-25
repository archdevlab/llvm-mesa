![image](https://user-images.githubusercontent.com/68618182/188527035-385752e7-fbd3-4865-abda-fdba4a804d99.png)

![image](https://user-images.githubusercontent.com/68618182/213734198-0cf50021-1f02-4c80-9a48-6f20ad42ce04.png)

# llvm-mesa

LLVM toolchain and Mesa packages for Archlinux. (git version) (No docs, no unittest, no test) (Less build time) Also included in this repo spirv-headers, spirv-tools, glslang, directx-headers, libdrm and libglvnd

### SPIRV-Headers and SPIRV-Tools

This repo contain spirv-headers-git and spirv-tools-git package. Why so? Because SPIRV-LLVM-Translator depends on it. Easier to have it in this repo and compile them at the same time of compiling LLVM.

### glslang

This repo contain glslang-git package. Why so? Because glslang depends on SPIRV-Headers and SPIRV-Tools. So it feel more natural to have this package in this repo.

### Directx-Headers

This repo contain directx-headers-git package. Why so? Because Mesa depends on it at build time. Mesa is the only package that depends on Directx-Headers at build time on Archlinux.

### libdrm

This repo contain libdrm-git and lib32-libdrm-git packages. Why so? Because Mesa depends on it at build time.

### libglvnd

This repo contain libglvnd-git and lib32-libglvnd-git packages. Why so? Because Mesa depends on it at build time.

### Mesa

Why Mesa's packages are not in a separate repo? Because mesa and llvm are closely tied together, everytime llvm changes/updates, mesa needs to be rebuilt. Another reason is that, everytimes glslang version change, archlinux recompile mesa against glslang new version, so it feel more natural to have this package in this repo. If you want to just compile Mesa's packages and not interested about LLVM, SPIRV and glslang package you can go ahead, no problem.

# Version

### SPIRV-Headers

- 1.4.304.0

- commit : 2b2e05e088841c63c0b6fd4c9fb380d8688738d3

### SPIRV-Tools

- 2024.4

- commit : 066c3d52c2fca8d9df79ca37055c3f5eddf2ffce

### glslang

- 15.1.0

- commit : 55627fd81dcc56068997073be9e3a2f912285904

### LLVM/LLD/COMPILRT-RT/CLANG/LLDB/LIBCLC

- 20.0.0

- commit: 335f1a72b22560e61f6170efef740c9c26b24f1a

### SPIRV-LLVM-Translator

- 20.0.0

- commit : cec12d6cf46306d0a015e883d5adb5a8200df1c0

### Directx-Headers

- 1.614.1

- commit : 358fbfca04e8c65784397d8184f161d64bfe569e

### libdrm

- 2.4.124

- commit : a7eb2cfd53a70fcd9ba9dcfad80a3994642f362f


### libglvnd

- 1.7.0

- commit : 606f6627cf481ee6dcb32387edc010c502cdf38b

### Mesa

- 25.0.0_devel

- commit : 4bed508122b20b750f71883f2225e14b9bb15102

# Build

    git clone https://github.com/archdevlab/llvm-mesa.git
    cd llvm-mesa
    ./build.sh

# Prebuild package

Prebuild package are available at https://repo.archdevlab.org/x86_64/llvm-mesa

You can add this repo to your pacman.conf

    [llvm-mesa]
    SigLevel = Optional TrustAll
    Server = https://repo.archdevlab.org/$arch/$repo

# Donation

BTC : bc1quz6zcjjy769cn9fd42r89hfh9unr4u2w4sfxer

ETH : 0xF8cBcA16f4eeDfF4a07D173B7fF53906a87b0476

DAI : 0xF8cBcA16f4eeDfF4a07D173B7fF53906a87b0476

LINK : 0xF8cBcA16f4eeDfF4a07D173B7fF53906a87b0476
