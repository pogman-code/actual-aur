# Maintainer: pikl <me@pikl.uk>
pkgname=actual-server
pkgver=24.12.0
pkgrel=1
pkgdesc="Actual Budget Server"
arch=('any')
url="https://github.com/actualbudget/actual-server"
license=('MIT')
groups=()
depends=('yarn' 'nodejs')
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
    "${pkgname}-${pkgver}.tar.gz::https://github.com/actualbudget/actual-server/archive/refs/tags/v${pkgver}.tar.gz"
    'actual-server.service'
    'sysusers'
    'tmpfiles'
    # Ensures the data directory is /var/lib/actual rather than
    # the project root (/usr/share/webapps/actual-server) which
    # is not writable by actual system user.
    # See following issue upstream:
    # https://github.com/actualbudget/actual/issues/2011#issuecomment-1837295607
    'load-config.js.patch'
)
noextract=()
sha256sums=('13f5b7b0486fc0839ab6a08fc594f8a00faae04f90ddeb1968d95c37bf6ebeec'
            'e52bafdd21264b7e809712f70731245326168a2e9f8fb7d1a3104703ab5821bc'
            '4dfa4502df8d72212ccfb96cfc2509c9a1461f542adb38304af54097b30ca0d5'
            'cba6a5df66a42ced857822e1099be00f2e37ec800f29cbbfca7210020140291b'
            '53027229d13857e60f10757224d2a6d6c2d68417c99fc6c878793606466e47ce')

prepare() {

    cd "${srcdir}/${pkgname}-${pkgver}"
    patch -p0 -i "${srcdir}/load-config.js.patch"
}

build() {

    cd "${srcdir}/${pkgname}-${pkgver}"
    yarn config set enableTelemetry 0
    yarn install
}

package() {

    install -d -m 0755 "${pkgdir}/usr/share/webapps/actual-server"
    cd "${srcdir}/${pkgname}-${pkgver}"
    cp -r {.,}* "${pkgdir}/usr/share/webapps/actual-server"

    install -d -m 0750 "${pkgdir}/var/lib/actual"

    cd "${srcdir}"
    install -D -m 0644 sysusers "${pkgdir}/usr/lib/sysusers.d/actual-server.conf"
    install -D -m 0644 tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/actual-server.conf"
    install -D -m 0644 actual-server.service "${pkgdir}/usr/lib/systemd/system/actual-server.service"
}
