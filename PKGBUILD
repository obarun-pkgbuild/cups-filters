# Maintainer: Eric Vidal <eric@obarun.org>
# based on the original https://git.archlinux.org/svntogit/packages.git/log/trunk?h=packages/cups-filters
# 						Maintainer: Andreas Radke <andyrtr@archlinux.org>

pkgname=cups-filters
pkgver=1.17.9
pkgrel=4
pkgdesc="OpenPrinting CUPS Filters"
arch=(x86_64)
url="https://wiki.linuxfoundation.org/openprinting/cups-filters"
license=('custom')
depends=('lcms2' 'poppler' 'qpdf' 'imagemagick' 'liblouis' 'ijs' 'libcups')
makedepends=('ghostscript' 'ttf-dejavu' 'python' 'mupdf-tools') # ttf-dejavu for make check
optdepends=('ghostscript: for non-PostScript printers to print with CUPS to convert PostScript to raster images'
	    'foomatic-db: drivers use Ghostscript to convert PostScript to a printable form directly'
	    'foomatic-db-engine: drivers use Ghostscript to convert PostScript to a printable form directly'
	    'foomatic-db-nonfree: drivers use Ghostscript to convert PostScript to a printable form directly'
	    'antiword: needed to convert MS Word documents (requires also docx2txt (AUR)')
backup=(etc/fonts/conf.d/99pdftoopvp.conf
        etc/cups/cups-browsed.conf)
source=(https://www.openprinting.org/download/cups-filters/$pkgname-$pkgver.tar.xz)
sha256sums=('98ae3f8c8309ea950144e582d252e65f4636e8812832a982edd0ab83eddef544')
validpgpkeys=('9acefd72f8e6a6e5fb6e80d37aad8c712c326bf484afb735e8ff7d7b86975870') # Eric Vidal

build() {
	export PYTHONPATH="/usr/share/glib-2.0"
  cd $pkgname-$pkgver
  ./configure --prefix=/usr  \
    --sysconfdir=/etc \
    --sbindir=/usr/bin \
    --localstatedir=/var \
    --with-rcdir=no \
    --enable-avahi \
    --with-browseremoteprotocols=DNSSD,CUPS \
    --enable-auto-setup-driverless \
    --with-test-font-path=/usr/share/fonts/TTF/DejaVuSans.ttf
  make
}

check() {
  cd $pkgname-$pkgver
  make check
}

package() {
  cd $pkgname-$pkgver
  make DESTDIR="$pkgdir/" install
  
  # use lp group from cups pkg FS#36769
  chgrp -R lp ${pkgdir}/etc/cups

  # license
  mkdir -p "${pkgdir}"/usr/share/licenses/${pkgname}
  install -m644 "${srcdir}"/${pkgname}-${pkgver}/COPYING "${pkgdir}"/usr/share/licenses/${pkgname}/
}
