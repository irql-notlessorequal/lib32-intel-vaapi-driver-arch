_pkgbasename=libva-intel-driver
pkgname=lib32-libva-intel-driver-irql
pkgver=2.4.3
pkgrel=1
pkgdesc='VA-API implementation for Intel G45 and HD Graphics family (32-bit)'
arch=('x86_64')
url='https://freedesktop.org/wiki/Software/vaapi'
license=('MIT')
depends=('lib32-libva')
provides=(lib32-libva-intel-driver)
conflicts=(lib32-libva-intel-driver)
source=(git+https://github.com/irql-notlessorequal/intel-vaapi-driver.git)
sha512sums=('SKIP')

pkgver() {
  cd intel-vaapi-driver
  cat VERSION
}

prepare() {
  cd intel-vaapi-driver

  # Only relevant if intel-gpu-tools is installed,
  # since then the shaders will be recompiled
  sed -i '1s/python$/&2/' src/shaders/gpp.py
}

build() {
  export CC="gcc -m32"
  export CXX="g++ -m32"
  export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"

  CFLAGS+=' -fcommon' # https://wiki.gentoo.org/wiki/Gcc_10_porting_notes/fno_common

  cd intel-vaapi-driver
  # Something isn't right on the 32-bit version, workaround this.
  ./autogen.sh
  
  ./configure \
    --prefix='/usr' \
    --libdir='/usr/lib32'
  make 
}

package() {
  cd intel-vaapi-driver

  make DESTDIR="${pkgdir}" install

  install -Dm644 COPYING "${pkgdir}"/usr/share/licenses/$pkgname/COPYING
}
