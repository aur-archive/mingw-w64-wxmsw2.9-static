# Maintainer: ÐÐ¸ÐºÐ¾Ð»Ð° Ð’ÑƒÐºÐ¾ÑÐ°Ð²Ñ™ÐµÐ²Ð¸Ñ› <hauzer@gmx.com>

pkgname=mingw-w64-wxmsw2.9-static
_pkgver_nomicro=2.9
pkgver=${_pkgver_nomicro}.5
pkgrel=3
pkgdesc='MSW implementation of wxWidgets API for GUI (mingw-w64, static)'
arch=('any')
url='http://wxwidgets.org'
license=('custom:wxWindows')
makedepends=('mingw-w64-gcc')
depends=('mingw-w64-crt' 'mingw-w64-libjpeg-turbo' 'mingw-w64-libpng' 'mingw-w64-expat' 'mingw-w64-libtiff')
options=(!strip !buildflags staticlibs)
conflicts=('mingw-w64-wxmsw' 'mingw-w64-wxmsw-static' 'mingw-w64-wxmsw2.9' 'mingw-w64-wxmsw2.9-static-with-debug')
provides=('mingw-w64-wxmsw' 'mingw-w64-wxmsw-static' 'mingw-w64-wxmsw2.9' 'mingw-w64-wxmsw2.9-static-with-debug')
source=("http://downloads.sourceforge.net/wxwindows/wxWidgets-${pkgver}.tar.bz2"
        'comdlg_filterspec_tdm-gcc.patch')
md5sums=('e98c5f92805493f150656403ffef3bb0'
         '00b2ac2be106cc040365e40ae55e162b')

_archs='i686-w64-mingw32 x86_64-w64-mingw32'

prepare() {
    cd "${srcdir}/wxWidgets-${pkgver}"

    # http://trac.wxwidgets.org/ticket/15545
    patch -p1 -i "${srcdir}/comdlg_filterspec_tdm-gcc.patch"
}

build() {
  for _arch in ${_archs} ; do
    unset LDFLAGS
    mkdir -p "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    "${srcdir}/wxWidgets-${pkgver}/configure" \
      --prefix=/usr/${_arch} \
      --build=$CHOST \
      --host=${_arch} \
      --with-msw \
      --disable-mslu \
      --enable-static \
      --disable-shared \
      --enable-iniconf \
      --enable-std_string_conv_in_wxstring
    make
  done
}

package() {
  for _arch in ${_archs} ; do
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    make DESTDIR="$pkgdir" install
    find "$pkgdir/usr/${_arch}" -name '*.exe' | xargs -rtl1 rm
    find "$pkgdir/usr/${_arch}" -name '*.a' | xargs -rtl1 ${_arch}-strip -g
    rm "$pkgdir/usr/${_arch}/bin/wx"{-config,rc-${_pkgver_nomicro}}
    ln -s "/usr/${_arch}/lib/wx/config/${_arch}-msw-unicode-static-${_pkgver_nomicro}" "$pkgdir/usr/${_arch}/bin/wx-config"
    rm -r "$pkgdir/usr/${_arch}/share"
  done
}

