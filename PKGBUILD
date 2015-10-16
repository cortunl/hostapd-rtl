pkgname=hostapd-rtl
_pkgname=hostapd
pkgver=2.5
pkgrel=1
pkgdesc="IEEE 802.11 AP, IEEE 802.1X/WPA/WPA2/EAP/RADIUS Authenticator with Realtek driver support"
arch=('any')
url="http://w1.fi/hostapd/"
license=('custom')
depends=('openssl' 'libnl')
options=(emptydirs)
source=(http://w1.fi/releases/$_pkgname-$pkgver.tar.gz
  config
  hostapd-2.3-noscan.patch
  openvswitch.patch
  driver_rtl.h
  driver_rtw.c
  rtlxdrv.patch
)
md5sums=(
  '69f9cec3f76d74f402864a43e4f8624f'
  'c7a6f2e564da36a7eb9253617243b9cc'
  'eaf8e48a9a63b5902fddadff2b8933fa'
  'a0802a604ed957078da0e14863df74f0'
  '1fb3502cba62d289e963f381563257fc'
  '21cde6b8f6fdd6968d1b761212a5ecaf'
  'ca8697f0254e3444f1e3758100c4f949'
)

prepare() {
  cd $_pkgname-$pkgver
  patch -p1 <$srcdir/hostapd-2.3-noscan.patch
  patch -p1 <$srcdir/openvswitch.patch
  patch -p1 <$srcdir/rtlxdrv.patch
}

build() {
  cd $_pkgname-$pkgver
  cp ../driver_rtl.h ./src/drivers
  cp ../driver_rtw.c ./src/drivers
  cd hostapd
  cp ../../config .config
  sed -i 's#/etc/hostapd#/etc/hostapd/hostapd#' hostapd.conf
  export CFLAGS="$CFLAGS $(pkg-config --cflags libnl-3.0)"
  make
}

package() {
  cd $_pkgname-$pkgver/hostapd
  install -d "$pkgdir/usr/bin"
  install -t "$pkgdir/usr/bin" hostapd hostapd_cli
  mv $pkgdir/usr/bin/hostapd $pkgdir/usr/bin/hostapd_rtl
  mv $pkgdir/usr/bin/hostapd_cli $pkgdir/usr/bin/hostapd_cli_rtl
}
