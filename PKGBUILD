# Maintainer: Vladislav Nepogodin <nepogodin.vlad@gmail.com>

pkgname=btop-git
pkgver=1.3.2.r1048.b48bf6a
pkgrel=1
pkgdesc="A monitor of resources"
arch=(x86_64 aarch64)
url="https://github.com/aristocratos/btop"
license=(Apache)
if [ -z "$TERMUX" ]; then
  depends=('gcc-libs')
else
  depends=()
fi
makedepends=('gcc' 'make' 'lowdown' 'git')
optdepends=(
  'nvidia-utils: NVIDIA GPU support'
  'rocm-smi-lib: AMD GPU support'
)
source=("${pkgname}::git+https://github.com/aristocratos/btop.git")
sha512sums=('SKIP')
provides=('btop')
conflicts=('btop')

if [ -z "$TERMUX" ]; then
  TERMUX_ROOT=''
else
  TERMUX_ROOT='/data/data/com.termux/files'
fi

pkgver() {
  cd "${srcdir}/${pkgname}"
  _pkgver="$(cat CHANGELOG.md | grep '^##' | sed 's/## v//g' | head -1)"

  printf "${_pkgver}.r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
  cd "${srcdir}/${pkgname}"

  git submodule init
  git config submodule."lib/fmt".url "${srcdir}/fmt"
  git -c protocol.file.allow=always submodule update

  # Patches
  patch --forward --strip=1 --input="${startdir}/feat-copy-cmd.patch"
  if [ -n "$TERMUX" ]; then
    patch --forward --strip=1 --input="${startdir}/pthread.patch"
  fi
}

build() {
  cd "${pkgname}"
  if [ -z "$TERMUX" ]; then
    make PLATFORM=linux GPU_SUPPORT=true RSMI_STATIC=false
    make all
  else
    export CXXFLAGS=''
    CXXFLAGS+=' -fstack-protector-strong'
    CXXFLAGS+=' -Oz'
    CXXFLAGS+=" -I${TERMUX_PREFIX}/include"

    export LDFLAGS="-Wl,-rpath=${TERMUX_ROOT}/usr/lib,--enable-new-dtags"

    make \
      DESTDIR="${pkgdir}${TERMUX_ROOT}" \
      ARCH=arm64 \
      CXX=aarch64-linux-android34-clang++ \
      PLATFORM=linux \
      all
  fi
}

package() {
  cd "${srcdir}/${pkgname}"
  if [ -z "$TERMUX" ]; then
    make DESTDIR="${pkgdir}${TERMUX_ROOT}" PREFIX=/usr PLATFORM=linux install
    make DESTDIR="${pkgdir}${TERMUX_ROOT}" PREFIX=/usr PLATFORM=linux setcap
  else
    make DESTDIR="${pkgdir}${TERMUX_ROOT}" PREFIX=/usr CXX=aarch64-linux-android34-clang++ PLATFORM=linux install
    make DESTDIR="${pkgdir}${TERMUX_ROOT}" PREFIX=/usr CXX=aarch64-linux-android34-clang++ PLATFORM=linux setcap
  fi

  if [ -n "$TERMUX" ]; then
    rm -rf ${pkgdir}${TERMUX_ROOT}/usr/share/applications/
    rm -rf ${pkgdir}${TERMUX_ROOT}/usr/share/icons/
  fi
}
