#Maintainer: Nicola Bignami <nicola@kernel-panic.no-ip.net>
#Contributor: Muhammed Uluyol <uluyol0@gmail.com>

pkgname=foo2zjs
pkgver=20150211
pkgrel=1
pkgdesc="foo2zjs Printer Drivers. Includes also foo2hp, foo2hbpl, foo2oak, foo2xqx, foo2qpdl, foo2slx, foo2hiperc and foo2lava drivers."
url="http://foo2zjs.rkkda.com/"
license=('GPL' 'custom')
depends=('psutils' 'cups' 'foomatic-db-engine' 'foomatic-db-foo2zjs')
conflicts=('foo2zjs-testing')
provides=('foo2zjs')
makedepends=('unzip' 'bc' 'wget')
optdepends=('tix: required by hplj10xx_gui.tcl')
arch=('i686' 'x86_64')
options=('!emptydirs' '!ccache')
install='foo2zjs.install'
source=("foo2zjs-${pkgver}.tar.gz::http://foo2zjs.rkkda.com/foo2zjs.tar.gz"
        'destdir-support-20140329-1.patch'
	'gen-fixes-20140329-1.patch'
	'firmware-loader-20130602-1.patch'
	'udev-firmware-loading-ruleset-20130601-1.patch')
md5sums=('1d468fd3513a32eb66bcf2c08f5f457c'
         '26ed4b81ba769a620a249b7395dc7057'
	 'c080b84ca44714eff986e3cf4f8d4bae'
	 'edfa38b48ae6429b957190d2c05a971b'
	 'ab84c888e05378c4618e764fe946d17a')
build() {
  cd "${srcdir}/${pkgname}"
  patch -p1 -i ${srcdir}/${source[1]} 
  patch -p1 -i ${srcdir}/${source[2]} 
  patch -p1 -i ${srcdir}/${source[3]}
  patch -p1 -i ${srcdir}/${source[4]}  
  make
}

package() {
  cd "${srcdir}/${pkgname}"
  for model in $(grep 'getone ' getweb.in | \
                 cut -d'#' -f1 | awk '{ print $2; }'); do
    if [[ $model != '$i' ]]; then
      ./getweb $model || true
    fi
  done

  install -d ${pkgdir}/usr/share/{applications,pixmaps,cups/model}
  install -d ${pkgdir}/usr/share/foomatic/db/source/{driver,opt,printer}

  make DESTDIR=${pkgdir} install install-hotplug-prog

  install -m755 getweb ${pkgdir}/usr/bin
  install -D -m644 COPYING "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
