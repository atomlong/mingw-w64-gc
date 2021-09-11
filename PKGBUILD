# Maintainer: Andrew Sun <adsun701 at gmail dot com>

pkgname=mingw-w64-gc
pkgver=8.0.4
pkgrel=1
pkgdesc="A garbage collector for C and C++ (mingw-w64)"
arch=('any')
url="https://www.hboehm.info/gc/"
license=('GPL')
makedepends=('mingw-w64-configure')
depends=('mingw-w64-crt' 'mingw-w64-libatomic_ops')
options=('!strip' '!buildflags' 'staticlibs')
source=("https://github.com/ivmai/bdwgc/releases/download/v${pkgver}/gc-${pkgver}.tar.gz")
sha256sums=('436a0ddc67b1ac0b0405b61a9675bca9e075c8156f4debd1d06f3a56c7cd289d')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

prepare() {
  cd "${srcdir}/gc-${pkgver}"
  autoreconf -fiv
}

build() {
  cd "${srcdir}/gc-${pkgver}"
  for _arch in ${_architectures}; do
    mkdir -p build-${_arch} && pushd build-${_arch}
    export lt_cv_deplibs_check_method='pass_all'
    CFLAGS+=" -fcommon" \
    ${_arch}-configure \
      --enable-threads=posix \
      --disable-dependency-tracking \
      --enable-large-config \
      --enable-cplusplus \
      ..
    make
    popd
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}/gc-${pkgver}/build-${_arch}"
    make DESTDIR="${pkgdir}" install
    rm -rf "$pkgdir"/usr/${_arch}/share
    ${_arch}-strip --strip-unneeded "$pkgdir"/usr/${_arch}/bin/*.dll
    ${_arch}-strip -g "$pkgdir"/usr/${_arch}/lib/*.a
  done
}
