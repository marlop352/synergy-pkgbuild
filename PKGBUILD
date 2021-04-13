# Maintainer: Marlus Lopes
# Contributor: Kevin MacMartin <prurigro@gmail.com>
# Contributor: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Jelle van der Waa <jelle vdwaa nl>
# Contributor: Stéphane Gaudreault <stephane@archlinux.org>
# Contributor: Dale Blount <dale@archlinux.org>
# Contributor: Michael Düll <mail@akurei.me>
# Contributor: Luca Corbatto <lucaatcorbatto.de>
# Ported from the upstream synergy package

_pkgname=synergy
pkgname=$_pkgname-marlus
pkgver=1.13.1.41
pkgrel=1
pkgdesc='Share a single mouse and keyboard between multiple computers'
url='https://symless.com/synergy/'
arch=('x86_64')
license=('GPL2')
provides=("$_pkgname")
conflicts=("$_pkgname")
depends=('gcc-libs' 'libxtst' 'libxinerama' 'libxkbcommon-x11' 'avahi' 'curl' 'openssl')
makedepends=('libxt' 'cmake' 'qt5-base' 'gmock' 'gtest' 'qt5-tools')
optdepends=('qt5-base: gui support')

source=(
  "$_pkgname::git+https://github.com/symless/$_pkgname-core.git#tag=$pkgver-stable"
  "$_pkgname.png"
  "${_pkgname}s_at.socket"
  "${_pkgname}s_at.service"
)

sha512sums=(
  'SKIP'
  'fc4db2f76a52d88d18a10a178ce885d618820a2a32fbde703e70e2000a54bc943d247064e9b0238fd13478dd59c8a1d85fdfafd9abbf80c6a7b45b0f321d84a0'
  'f9c124533dfd0bbbb1b5036b7f4b06f7f86f69165e88b9146ff17798377119eb9f1a4666f3b2ee9840bc436558d715cdbfe2fdfd7624348fae64871f785a1a62'
  'e85cc3452bb8ba8fcccb1857386c77eb1e4cabb149a1c492c56b38e1b121ac0e7d96c6fcbd3c9b522d3a4ae9d7a9974f4a89fc32b02a56f665be92af219e371c'
)

build() {
  # Build synergy
  cd $_pkgname
  export SYNERGY_REVISION=$(git rev-parse --short HEAD)
  export GIT_COMMIT=$(git rev-parse HEAD)
  export CMAKE_BUILD_TYPE=Release
  export SYNERGY_ENTERPRISE=ON
  cmake -DCMAKE_INSTALL_PREFIX=/usr .
  make
}

check() {
  # Run tests
  cd $_pkgname
  ./bin/unittests
  ./bin/integtests
}

package() {
  # Install systemd service and socket
  install -Dm644 ${_pkgname}s_at.service "$pkgdir/usr/lib/systemd/system/${_pkgname}s@.service"
  install -Dm644 ${_pkgname}s_at.socket "$pkgdir/usr/lib/systemd/system/${_pkgname}s@.socket"

  # Install binary
  cd $_pkgname
  install -Dm755 bin/${_pkgname} "$pkgdir/usr/bin/${_pkgname}"
  install -Dm755 bin/${_pkgname}c "$pkgdir/usr/bin/${_pkgname}c"
  install -Dm755 bin/${_pkgname}d "$pkgdir/usr/bin/${_pkgname}d"
  install -Dm755 bin/${_pkgname}s "$pkgdir/usr/bin/${_pkgname}s"
  install -Dm755 bin/syntool "$pkgdir/usr/bin/syntool"

  # Install config
  install -Dm644 doc/$_pkgname.conf.example "$pkgdir/etc/$_pkgname.conf.example"
  install -Dm644 doc/$_pkgname.conf.example-advanced "$pkgdir/etc/$_pkgname.conf.example-advanced"
  install -Dm644 doc/$_pkgname.conf.example-basic "$pkgdir/etc/$_pkgname.conf.example-basic"

  # Install manfiles
  install -Dm644 doc/${_pkgname}c.man "$pkgdir/usr/share/man/man1/${_pkgname}c.1"
  install -Dm644 doc/${_pkgname}s.man "$pkgdir/usr/share/man/man1/${_pkgname}s.1"
}
