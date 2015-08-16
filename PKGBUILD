# $Id$
# Maintainer: Patryk Kowalczyk <patryk AT kowalczyk DOT ws>

pkgname=imagemagick-opencl
pkgver=6.8.6.7
pkgrel=1
pkgdesc="An image viewing/manipulation program with OpenCL and OpenMP"
arch=('i686' 'x86_64')
url="http://www.imagemagick.org/"
license=('custom')
depends=('libtool' 'lcms2' 'libxt' 'gcc-libs' 'bzip2' 'freetype2' 'fontconfig' 'libxext' 'perl' 'librsvg' 'liblqr')
makedepends=('ghostscript>=9.00' 'openexr' 'libwmf' 'libxml2' 'jasper' 'libpng')
optdepends=('ghostscript: for Ghostscript support' 
            'openexr: for OpenEXR support' 
            'libwmf: for WMF support' 
            'librsvg: for SVG support'
            'libxml2: for XML support'
            'jasper: for JPEG-2000 support'
	    'libpng: for PNG support')
options=('!makeflags' '!docs' 'libtool')
backup=('etc/ImageMagick/coder.xml'
          'etc/ImageMagick/colors.xml'
          'etc/ImageMagick/delegates.xml'
          'etc/ImageMagick/log.xml'
          'etc/ImageMagick/magic.xml'
          'etc/ImageMagick/mime.xml'
          'etc/ImageMagick/policy.xml'
          'etc/ImageMagick/quantization-table.xml'
          'etc/ImageMagick/thresholds.xml'
          'etc/ImageMagick/type.xml'
          'etc/ImageMagick/type-dejavu.xml'
          'etc/ImageMagick/type-ghostscript.xml'
          'etc/ImageMagick/type-windows.xml')
source=(ftp://ftp.imagemagick.org/pub/ImageMagick/ImageMagick-${pkgver%.*}-${pkgver##*.}.tar.xz \
	perlmagick.rpath.patch)
    
replaces=(imagemagick)
conflicts=(imagemagick)
provides=(imagemagick)
build() {
	cd "${srcdir}"/ImageMagick-${pkgver%.*}-${pkgver##*.}
	sed '/AC_PATH_XTRA/d' -i configure.ac
        autoreconf --force --install
	patch -p0 < ../perlmagick.rpath.patch

	./configure --prefix=/usr --with-modules --disable-static \
	    --enable-openmp --with-x --with-wmf --with-openexr --with-xml \
	    --with-lcms2 --with-gslib --with-gs-font-dir=/usr/share/fonts/Type1 \
	    --with-perl --with-perl-options="INSTALLDIRS=vendor" \
	    --without-gvc --without-djvu --without-autotrace --with-jp2 --with-rsvg\
	    --without-jbig --without-fpx --without-dps --enable-opencl --enable-hdri
	make
}

package() {
	cd "${srcdir}"/ImageMagick-${pkgver%.*}-${pkgver##*.}

	make DESTDIR="${pkgdir}" install

	install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
	install -Dm644 NOTICE "${pkgdir}/usr/share/licenses/${pkgname}/NOTICE"

	#Cleaning
	find "${pkgdir}" -name '*.bs' -exec rm {} \;
	rm -f "${pkgdir}"/usr/lib/*.la
}

md5sums=('2881374dceee2becf5c708911bd7830d'
         'ff9974decbfe9846f8e347239d87e4eb')
