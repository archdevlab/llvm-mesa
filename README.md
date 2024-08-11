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

- 1.5.5

- commit : f013f08e4455bcc1f0eed8e3dd5e2009682656d9

### SPIRV-Tools

- 2024.3

- commit : 246daf246bb17336afcf4482680bba434b1e5557

### glslang

- 14.3.0

- commit : 5398d55e33dff7d26fecdd2c35808add986c558c

### LLVM/LLD/COMPILRT-RT/CLANG/LLDB/LIBCLC

- 20.0.0

- commit: e76028a11fd3195a3170c1b0cb308e62e73a05a3

### SPIRV-LLVM-Translator

- 20.0.0

- commit : 88e546a689b26792feff6c84e049b13e2e0feef8

### Directx-Headers

- 1.613.1

- commit : 9be295b3b81ce1d0ff2b44f18d0eb86ea54c5122

### libdrm

- 2.4.122

- commit : 88db6114985ebcfe14f592930d82d01a3d973101


### libglvnd

- 1.7.0

- commit : 606f6627cf481ee6dcb32387edc010c502cdf38b

### Mesa

- 24.3.0_devel

- commit : d9849ac46623797a9f56fb9d46dc52460ac477de

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
