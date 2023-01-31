# Maintainer: Vladislav Nepogodin <nepogodin.vlad@gmail.com>

pkgname=btop-git
pkgver=1.2.13.r664.584647a
pkgrel=1
pkgdesc="A monitor of resources"
arch=(any)
url="https://github.com/aristocratos/btop"
license=('Apache-2.0')
depends=('libxcb')
makedepends=('gcc' 'make' 'git')
source=("mirrors/btop::git+https://github.com/stevenxxiu/btop.git#branch=feat/copy-cmd")
sha512sums=('SKIP')
provides=('btop')
conflicts=('btop')
options=(!strip)

pkgver() {
  cd "${srcdir}/btop"
  _pkgver="$(cat CHANGELOG.md | grep '^##' | sed 's/## v//g' | head -1)"

  printf "${_pkgver}.r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  cd "${srcdir}/btop"
  make
}

package() {
  cd "${srcdir}/btop"
  DESTDIR="${pkgdir}" make install

  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/btop/LICENSE"
}
