# Maintainer: Vladislav Nepogodin <nepogodin.vlad@gmail.com>

pkgname=btop-git
pkgver=1.3.2.r1032.d168073
pkgrel=1
pkgdesc="A monitor of resources"
arch=(x86_64)
url="https://github.com/aristocratos/btop"
license=('Apache-2.0')
depends=('gcc-libs')
makedepends=('gcc' 'make' 'lowdown' 'git')
optdepends=(
  'nvidia-utils: NVIDIA GPU support'
  'rocm-smi-lib: AMD GPU support'
)
source=("${pkgname}::git+https://github.com/aristocratos/btop.git")
sha512sums=('SKIP')
provides=('btop')
conflicts=('btop')

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
}

build() {
  cd "${pkgname}"

  make PLATFORM=linux GPU_SUPPORT=true RSMI_STATIC=false
}

package() {
  cd "${srcdir}/${pkgname}"
  DESTDIR="${pkgdir}" make PREFIX=/usr PLATFORM=linux install

  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

# vim:set sw=2 sts=2 et:
