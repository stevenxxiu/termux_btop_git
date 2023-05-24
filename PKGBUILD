# Maintainer: Vladislav Nepogodin <nepogodin.vlad@gmail.com>

pkgname=btop-git
pkgver=1.2.13.r689.4689938
pkgrel=1
pkgdesc="A monitor of resources"
arch=(any)
url="https://github.com/aristocratos/btop"
license=('Apache-2.0')
makedepends=('gcc' 'make' 'git')
source=("mirrors/btop::git+https://github.com/aristocratos/btop.git"
        "mirrors/fmt::git+https://github.com/fmtlib/fmt.git")
sha512sums=('SKIP'
            'SKIP')
provides=('btop')
conflicts=('btop')
options=(!strip)

pkgver() {
  cd "${srcdir}/btop"
  _pkgver="$(cat CHANGELOG.md | grep '^##' | sed 's/## v//g' | head -1)"

  printf "${_pkgver}.r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
  cd "${srcdir}/btop"
  git submodule init

  git config submodule.lib/fmt.url "${srcdir}/fmt"
  git -c protocol.file.allow=always submodule update lib/fmt

  # Patches
  cd "${srcdir}/btop"
  patch --forward --strip=1 --input="${startdir}/feat-copy-cmd.patch"
}

build() {
  cd "${srcdir}/btop"

  make PLATFORM=linux STATIC=true
}

package() {
  cd "${srcdir}/btop"
  DESTDIR="${pkgdir}" make PLATFORM=linux install

  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/btop/LICENSE"
}
