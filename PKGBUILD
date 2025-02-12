# Maintainer: pikl <me@pikl.uk>
# Maintainer: POGMAN <adrian.maurin@gmail.com>

# Official Documentation: https://actualbudget.org/docs/install/
pkgname=actual
pkgver=25.3.0
pkgrel=1
pkgdesc="Actual Budget"
arch=('any')
url="https://github.com/actualbudget/actual"
license=('MIT')
groups=()
depends=('yarn' 'nodejs' 'npm')
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
    "${pkgname}-${pkgver}.tar.gz::${url}/archive/refs/heads/v${pkgver}.tar.gz"
    'actual.service'
    'sysusers'
    'tmpfiles'
    # Ensures the data directory is /var/lib/actual rather than
    # the project root (/usr/share/webapps/actual) which
    # is not writable by actual system user.
    # See following issue upstream:
    # https://github.com/actualbudget/actual/issues/2011#issuecomment-1837295607
    'load-config.js.patch'
)
noextract=()
sha256sums=('3c73f8c59ddc31d74aba73a4506432370bc135a75ed6aea44b23ed3bb576ea7c'
            'e76d13ac327c111e12a552c698796fe845605d431734c79c99b65a41e5d8ce9d'
            '4dfa4502df8d72212ccfb96cfc2509c9a1461f542adb38304af54097b30ca0d5'
            'cba6a5df66a42ced857822e1099be00f2e37ec800f29cbbfca7210020140291b'
            '98bf6d75bbdc56d447f1766dd9a251422306cd6674d6e5c18e0ddb6ab4244729')

prepare() {
    cd "${srcdir}/${pkgname}-${pkgver}"
    patch -p0 -i "${srcdir}/load-config.js.patch"
}

build() {
    cd "${srcdir}/${pkgname}-${pkgver}"
    yarn config set enableTelemetry 0
    yarn install
    yarn install:server
}

package() {
    install -d -m 0755 "${pkgdir}/usr/share/webapps/actual"
    cd "${srcdir}/${pkgname}-${pkgver}"
    cp -r {.,}* "${pkgdir}/usr/share/webapps/actual"

    install -d -m 0750 "${pkgdir}/var/lib/actual"

    cd "${srcdir}"
    install -D -m 0644 sysusers "${pkgdir}/usr/lib/sysusers.d/actual.conf"
    install -D -m 0644 tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/actual.conf"
    install -D -m 0644 actual.service "${pkgdir}/usr/lib/systemd/system/actual.service"
}
