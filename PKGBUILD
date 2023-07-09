# Maintainer: Chris <chris@pikl.uk>
pkgname=actual-server
pkgver=v23.7.1
pkgrel=7
pkgdesc="Actual Budget Server"
arch=('any')
url="https://github.com/actualbudget/actual-server"
license=('MIT')
groups=()
depends=('yarn' 'nodejs-lts-hydrogen') 
makedepends=('git' 'gcc' 'make')
optdepends=()
provides=()
conflicts=()
replaces=()
backup=()
options=('!strip')
install=
changelog=
source=(
    "git+https://github.com/actualbudget/$pkgname#tag=$pkgver"
    'actual-server.service'
    'sysusers'
    'tmpfiles'
)
noextract=()
sha256sums=('SKIP'
            'b2d7e042688726b5ad374fbb08ae5ef736b31702f611595d337226981394c082'
            '4dfa4502df8d72212ccfb96cfc2509c9a1461f542adb38304af54097b30ca0d5'
            '0cd5cd3b9a3bfcc9c572cd42d11d1e5d64be9cf979527612b8874baf56ddd5ef')

build() {

    cd "$srcdir/$pkgname"
    yarn config set enableTelemetry 0
    yarn install
}

package() {

    install -d -m 0755 "$pkgdir/usr/share/webapps"
    install -d -m 0750 "$pkgdir/usr/share/webapps/actual-server"
    install -d -m 0750 "$pkgdir/var/lib/actual"

    cd "$srcdir/$pkgname"
    cp -r * "$pkgdir/usr/share/webapps/actual-server"

    cd "$srcdir"
    install -D -m 0644 sysusers "$pkgdir/usr/lib/sysusers.d/actual-server.conf"
    install -D -m 0644 tmpfiles "$pkgdir/usr/lib/tmpfiles.d/actual-server.conf"
    install -D -m 0644 actual-server.service "$pkgdir/usr/lib/systemd/system/actual-server.service"
}
