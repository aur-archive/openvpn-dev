# Contributor: Thomas Bächler <thomas@archlinux.org>
# Maintainer: Clément DEMOULINS <clement@archivel.fr>

pkgname=openvpn-dev
_pkgname=openvpn
pkgver=2.3.6
pkgrel=1
pkgdesc="An easy-to-use, robust, and highly configurable VPN (Virtual Private Network)"
arch=('i686' 'x86_64')
url="http://openvpn.net/index.php/open-source.html"
license=('custom')
depends=('openssl' 'lzo2' 'iproute2')
provides=('openvpn')
conflicts=('openvpn')
options=('!libtool')
optdepends=('openresolv: scripts can use this tool to manage resolv.conf')

source=(http://swupdate.openvpn.net/community/releases/${_pkgname}-${pkgver}.tar.xz
        http://swupdate.openvpn.net/community/releases/${_pkgname}-${pkgver}.tar.xz.asc
        openvpn@.service)
md5sums=('bcc30c296566df14feebdd8aa0e408ca'
         'SKIP'
         '44047df812a3fcd57a7e36a61732a9b9')
validpgpkeys=('03300E11FED16F59715F9996C29D97ED198D22A3')

build() {
  cd "$srcdir/${_pkgname}-${pkgver}"

  # Build openvpn
  CFLAGS="$CFLAGS -DPLUGIN_LIBDIR=\\\"/usr/lib/openvpn\\\"" ./configure \
    --prefix=/usr \
    --sbindir=/usr/bin \
    --enable-password-save \
    --enable-systemd \
    --enable-iproute2 \
    --mandir=/usr/share/man
  make
}

package() {
  cd "$srcdir/${_pkgname}-${pkgver}"

  # Install openvpn
  make DESTDIR="$pkgdir" install distdir
  install -d -m755 "$pkgdir"/etc/openvpn
  install -m 644 -D "$srcdir/openvpn@.service" "$pkgdir/usr/lib/systemd/system/openvpn@.service"

  # Install samples
  for sample in sample-config-files sample-keys sample-plugins sample-scripts sample-windows; do
    cp -r "sample/${sample}" "${pkgdir}/usr/share/doc/openvpn/sample/"
  done
  cp -r contrib "${pkgdir}/usr/share/doc/openvpn/"

  # Install license
  install -D -m644 COPYING "$pkgdir/usr/share/licenses/$pkgname/COPYING"
}

# vim:set ts=2 sw=2 et:
