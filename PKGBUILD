# _     _            _        _          _____
#| |__ | | __ _  ___| | _____| | ___   _|___ /
#| '_ \| |/ _` |/ __| |/ / __| |/ / | | | |_ \
#| |_) | | (_| | (__|   <\__ \   <| |_| |___) |
#|_.__/|_|\__,_|\___|_|\_\___/_|\_\\__, |____/
#                                  |___/

#Maintainer: blacksky3 <https://github.com/blacksky3>
#Credits: Evangelos Foutras <evangelos@foutrelis.com>
#Credits: Jan "heftig" Steffens <jan.steffens@gmail.com>

pkgbase=lib32-llvm-minimal-git
pkgname=(lib32-llvm-minimal-git lib32-llvm-libs-minimal-git)
url='https://llvm.org/'
pkgver=16.0.0.r449221.g49d47c4d2f28
pkgrel=1
_pkgver=16.0.0
commit=49d47c4d2f280d15d1de94c53b72b6ab3c127b35
arch=(i686 x86_64)
license=('custom:Apache 2.0 with LLVM Exception')
makedepends=(cmake ninja lib32-libffi lib32-zlib lib32-zstd python gcc-multilib lib32-libxml2 git patch)
options=(staticlibs !lto ) # extra/llvm has many test failures with LTO
source=(git+https://github.com/llvm/llvm-project.git#commit=$commit)

# Both ninja & LIT by default use all available cores. this can lead to heavy stress on systems making them unresponsive.
# It can also happen that the kernel oom killer interferes and kills important tasks.
# A reasonable value for them to avoid these issues appears to be 75% of available cores.
# NINJAFLAGS and LITFLAGS are env vars that can be used to achieve this. They should be set on command line or in files read by your shell on login (like .bashrc ) .
# example for systems with 24 cores
NINJAFLAGS="-j 20 -l 20"
# LITFLAGS="-j 18"
# NOTE: It's your responbility to validate the value of NINJAFLAGS and LITFLAGS. If unsure, don't set it.

# Utilizing LLVM_DISTRIBUTION_COMPONENTS to avoid
# installing static libraries; inspired by Gentoo
_get_distribution_components(){
  local target
  ninja -t targets | grep -Po 'install-\K.*(?=-stripped:)' | while read -r target; do
    case $target in
      clang-libraries|distribution)
        continue
        ;;
      clang-tidy-headers)
        continue
        ;;
      clang|clangd|clang-*)
        ;;
      clang*|findAllSymbols)
        continue
        ;;
    esac
    echo $target
  done
}

pkgver(){
  cd ${srcdir}/llvm-project/llvm

  # This will almost match the output of `llvm-config --version` when the
  # LLVM_APPEND_VC_REV cmake flag is turned on. The only difference is
  # dash being replaced with underscore because of Pacman requirements.
  local _pkgver=$(awk -F 'MAJOR |MINOR |PATCH |)' \
  'BEGIN { ORS="." ; i=0 } \
  /set\(LLVM_VERSION_/ { print $2 ; i++ ; if (i==2) ORS="" } \
  END { print "\n" }' \
  CMakeLists.txt).r$(git rev-list --count HEAD).g$(git rev-parse --short HEAD)
  echo "$_pkgver"
}

build(){
export CC='gcc -m32'
export CXX='g++ -m32'
export ASFLAGS=--32
export CFLAGS=-m32
export CXXFLAGS=-m32
export PKG_CONFIG_PATH=/usr/lib32/pkgconfig

  cd ${srcdir}/llvm-project/llvm

  #rm -rf build

  #mkdir build

  cd build

  local cmake_args=(
    -G Ninja
    -DCMAKE_CXX_FLAGS:STRING=-m32
    -DCMAKE_C_FLAGS:STRING=-m32
    -DLLVM_ENABLE_PROJECTS="compiler-rt;clang-tools-extra;clang"
    -DCOMPILER_RT_INSTALL_PATH=/usr/lib/clang/$_pkgver
    -DCMAKE_BUILD_TYPE=Release
    -DCMAKE_INSTALL_PREFIX=/usr
    -DLLVM_LIBDIR_SUFFIX=32
    -DLLVM_TARGET_ARCH:STRING=i686
    -DLLVM_HOST_TRIPLE=$CHOST
    -DLLVM_DEFAULT_TARGET_TRIPLE="i686-pc-linux-gnu"
    -DLLVM_TARGETS_TO_BUILD="AMDGPU;X86"
    -DLLVM_BUILD_LLVM_DYLIB=ON
    -DLLVM_LINK_LLVM_DYLIB=ON
    -DLLVM_ENABLE_RTTI=ON
    -DLLVM_ENABLE_FFI=ON
    -DLLVM_USE_PERF=ON
    -DLLVM_INCLUDE_BENCHMARKS=OFF
    -DLLVM_INCLUDE_EXAMPLES=OFF
    -DLLVM_BUILD_DOCS=OFF
    -DLLVM_INCLUDE_DOCS=OFF
    -DLLVM_ENABLE_SPHINX=OFF
    -DLLVM_ENABLE_OCAMLDOC=OFF
    -DLLVM_ENABLE_DOXYGEN=OFF
    -DFFI_INCLUDE_DIR=$(pkg-config --variable=includedir libffi)
    -DLLVM_BINUTILS_INCDIR=/usr/include
    -DLLVM_VERSION_SUFFIX=""
    -DLLVM_ENABLE_BINDINGS=OFF
    -DLLVM_LIT_ARGS="$LITFLAGS"" -sv --ignore-fail"
    -Wno-dev
  )

  cmake .. "${cmake_args[@]}"
  local distribution_components=$(_get_distribution_components | paste -sd\;)
  test -n "$distribution_components"
  cmake_args+=(-DLLVM_DISTRIBUTION_COMPONENTS="$distribution_components")

  cmake .. "${cmake_args[@]}"
  ninja $NINJAFLAGS
}

package_lib32-llvm-minimal-git(){
  pkgdesc='Collection of modular and reusable compiler and toolchain technologies (32-bit) (git version)'
  depends=(lib32-llvm-libs llvm)
  conflicts=(lib32-llvm)
  provides=(lib32-llvm)

  DESTDIR="$pkgdir" ninja $NINJAFLAGS -C ${srcdir}/llvm-project/llvm/build install
  #DESTDIR="$pkgdir" ninja $NINJAFLAGS -C ${srcdir}/llvm-project/llvm/build install-distribution

  # The runtime library goes into lib32-llvm-libs
  mv "$pkgdir"/usr/lib32/lib{LLVM,LTO,Remarks}*.so* "$srcdir"
  mv -f "$pkgdir"/usr/lib32/LLVMgold.so "$srcdir"

  # Fix permissions of static libs
  chmod -x "$pkgdir"/usr/lib32/*.a

  mv "$pkgdir/usr/bin/llvm-config" "$pkgdir/usr/lib32/llvm-config"
  mv "$pkgdir/usr/include/llvm/Config/llvm-config.h" "$pkgdir/usr/lib32/llvm-config-32.h"

  rm -rf "$pkgdir"/usr/{bin,include,lib,libexec,share/{doc,man,llvm,opt-viewer}}

  # Needed for multilib (https://bugs.archlinux.org/task/29951)
  # Header stub is taken from Fedora
  install -d "$pkgdir/usr/include/llvm/Config"
  mv "$pkgdir/usr/lib32/llvm-config-32.h" "$pkgdir/usr/include/llvm/Config/"

  mkdir "$pkgdir"/usr/bin
  mv "$pkgdir/usr/lib32/llvm-config" "$pkgdir/usr/bin/llvm-config32"

  install -Dm644 "$srcdir/llvm-project/llvm/LICENSE.TXT" "$pkgdir/usr/share/licenses/$pkgname/llvm/LICENSE"
  install -Dm644 "$srcdir/llvm-project/llvm/CREDITS.TXT" "$pkgdir/usr/share/licenses/$pkgname/llvm/CREDITS"
  install -Dm644 "$srcdir/llvm-project/llvm/CODE_OWNERS.TXT" "$pkgdir/usr/share/licenses/$pkgname/llvm/CODE_OWNERS"

  install -Dm644 "$srcdir/llvm-project/clang/LICENSE.TXT" "$pkgdir/usr/share/licenses/$pkgname/clang/LICENSE"
  install -Dm644 "$srcdir/llvm-project/clang/CodeOwners.rst" "$pkgdir/usr/share/licenses/$pkgname/clang/CodeOwners.rst"
}

package_lib32-llvm-libs-minimal-git(){
  pkgdesc='Low Level Virtual Machine (runtime library) (32-bit) (git version)'
  depends=(lib32-libffi lib32-zlib lib32-zstd lib32-ncurses lib32-libxml2 lib32-gcc-libs)
  conflicts=(lib32-llvm-libs)
  provides=(lib32-llvm-libs)

  install -d "$pkgdir/usr/lib32"

  cp -P \
    "$srcdir"/lib{LLVM,LTO,Remarks}*.so* \
    "$srcdir"/LLVMgold.so \
    "$pkgdir/usr/lib32/"

  # Symlink LLVMgold.so from /usr/lib/bfd-plugins
  # https://bugs.archlinux.org/task/28479
  install -d "$pkgdir/usr/lib32/bfd-plugins"
  ln -s ../LLVMgold.so "$pkgdir/usr/lib32/bfd-plugins/LLVMgold.so"

  install -Dm644 "$srcdir/llvm-project/llvm/LICENSE.TXT" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm644 "$srcdir/llvm-project/llvm/CREDITS.TXT" "$pkgdir/usr/share/licenses/$pkgname/CREDITS"
  install -Dm644 "$srcdir/llvm-project/llvm/CODE_OWNERS.TXT" "$pkgdir/usr/share/licenses/$pkgname/CODE_OWNERS"
}

sha256sums=('SKIP')
