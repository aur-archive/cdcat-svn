# Maintainer : uzsolt <udvzsolt gmail com>
# Contributor : SpepS <dreamspepser at yahoo dot it>
# Contributor: Wes Brewer <brewerw@gmail.com>

pkgname=cdcat-svn
pkgver=20120402
pkgrel=3
pkgdesc="CD/DVD/Media catalog software (QT based)"
arch=('i686' 'x86_64')
url="http://cdcat.sourceforge.net/"
license=('GPL')
depends=('qt4' 'bzip2' 'libexif' 'p7zip')
makedepends=('libtar' 'lib7zip' 'libmediainfo' 'subversion')
provides=("cdcat")
conflicts=("cdcat")
install="cdcat.install"
source=("cdcat.desktop")
md5sums=('919c7e03e085a1af5a1e4d30075e30a7')

_pkgname=cdcat

build() {
  cd "$srcdir"
  msg "Connecting to SVN server over HTTP..."
  svn co http://cdcat.svn.sourceforge.net/svnroot/cdcat cdcat
  cd cdcat
  [ -d build ] || mkdir build
  cd build

  sed -i "s|/local||g ; s|-ltar|/usr/lib/libtar.a|" ../trunk/src/${_pkgname}.pro
  # remove catalog encryption support
  # if you want it please add libcrypto++ do depends and disable next line
  sed -i "/CATALOG_ENCRYPTION/d ; /lcrypto++/d" ../trunk/src/${_pkgname}.pro

  qmake-qt4 ../trunk/src/${_pkgname}.pro
  make
}

package() {
  cd "$srcdir/${_pkgname}/build"

  make INSTALL_ROOT="$pkgdir" install

  # desktop file
  install -Dm644 "$srcdir/${_pkgname}.desktop" \
    "$pkgdir/usr/share/applications/${_pkgname}.desktop"

  # icons
  for _s in 16x16 22x22 32x32 48x48 64x64; do
    install -Dm644 $srcdir/${_pkgname}/trunk/${_pkgname}_logo_$_s.png \
      "$pkgdir/usr/share/icons/hicolor/$_s/apps/${_pkgname}.png"
  done
  install -Dm644 $srcdir/${_pkgname}/trunk/${_pkgname}_logo.svg \
    "$pkgdir/usr/share/icons/hicolor/scalable/apps/$_pkgname.png"

  # translations
  cd $srcdir/${_pkgname}/trunk/src/lang
  for _f in *.ts; do
    _tdir="$pkgdir/usr/share/locale/${_f:6:2}/LC_MESSAGES"
    install -d "$_tdir"
    lrelease-qt4 -silent -qm "$_tdir/${_f/ts/qm}" $_f
  done
}
# vim:set ts=2 sw=2 et:
