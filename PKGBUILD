# Maintainer: pikl <me@pikl.uk>
# Maintainer: POGMAN <adrian.maurin@gmail.com>

# Official Documentation: https://actualbudget.org/docs/install/
pkgname=actual
pkgver=master
pkgrel=1
pkgdesc="Actual Budget"
arch=('any')
url="https://github.com/actualbudget/actual"
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
    "${pkgname}-${pkgver}.tar.gz::${url}/archive/refs/heads/${pkgver}.tar.gz"
    'actual-server.service'
    'actual.sysusers'
    'actual.tmpfiles'
    'actual.conf'
)
noextract=()
sha256sums=('3c73f8c59ddc31d74aba73a4506432370bc135a75ed6aea44b23ed3bb576ea7c'
            'b7cde49b3406e4809708286dd1668cefecc64bb7f849d3d87326f92a3554568c'
            '8972650fe50b14b543278d243d0bfe7ac655cdd5b4c63ce92a7ad27c835764e7'
            'cba6a5df66a42ced857822e1099be00f2e37ec800f29cbbfca7210020140291b'
            '81a69c3376a1470c2f30aea4ebb3a354cf3c6a14679fa676e427e8b144d29f7c')
backup=('etc/conf.d/actual')


build() {
    cd "${srcdir}/${pkgname}-${pkgver}"
    yarn config set enableTelemetry 0
    yarn workspaces focus actual-sync --production
}

package() {
    install -d -m 0755 "${pkgdir}/usr/share/webapps/actual"
    cd "${srcdir}/${pkgname}-${pkgver}"
    cp -r {.,}* "${pkgdir}/usr/share/webapps/actual"

    install -d -m 0750 "${pkgdir}/var/lib/actual"

    cd "${srcdir}"
    install -D -m 0644 actual.sysusers "${pkgdir}/usr/lib/sysusers.d/actual.conf"
    install -D -m 0644 actual.tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/actual.conf"
    install -D -m 0644 actual-server.service "${pkgdir}/usr/lib/systemd/system/actual-server.service"
    install -D -m 0644 actual.conf "$pkgdir"/etc/conf.d/actual

}
