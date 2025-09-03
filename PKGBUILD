# Maintainer: Carlo Buccisano <archlinux AT bcarlo.mozmail.com>
pkgname="cnijfilter2"
pkgver="5.50"
pkgrel="1"
pkgdesc="Cups driver for printer Canon PIXMA TS5100 series"
arch=("x86_64")
url="https://www.canon.co.uk/support/consumer/products/printers/pixma/ts-series/pixma-ts5151.html?type=drivers&os=Linux%20(64-bit)"
license=('GPL' 'custom:canon')
depends=('cups' 'libcups')
makedepends=('automake' 'autoconf')
provides=('cmdtocanonij2' 'cmdtocanonij3' 'cnijbe2' 'lgmon3' 'rastertocanonij' 'tocanonij' 'tocnpwg')
conflicts=('cnijfilter' 'cnijfilter-mg3600')
source=("${pkgname}-${pkgver}.tar.gz::http://pdisp01.c-wss.com/gdl/WWUFORedirectTarget.do?id=MDEwMDAwOTExMDAx&cmp=ABX&lang=EN")
sha256sums=('SKIP')
prepare() {
	cd "$pkgname-source-$pkgver-$pkgrel"
	sed -i '39 a #include <stdlib.h>' 'lgmon3/src/keytext.c'    # They use free() without including stdlib
	sed -i '50,52d' 'lgmon3/src/common/libcnnet2_type.h'	    # They redefine bool type, which raises an error with GCC
	sed -e '/GET_PROTOCOL/ s:^int:extern &:g' -i 'lgmon3/src/cnijlgmon3.c'   # They define multiple times the function GET_PROTOCOL, so we use extern
}

build() {
	cd "$pkgname-source-$pkgver-$pkgrel"
	
	pushd cmdtocanonij2
		./autogen.sh --prefix=/usr \
			--datadir=/usr/share \
			LDFLAGS="-L../../com/libs_bin64"
		make
	popd

	pushd cmdtocanonij3
		./autogen.sh --prefix=/usr \
			--datadir=/usr/share \
			LDFLAGS="-L../../com/libs_bin64"
		make 
	popd

	pushd cnijbe2
		./autogen.sh --prefix=/usr \
			--enable-progpath=/usr/bin
		make
	popd

	pushd lgmon3
		./autogen.sh --prefix=/usr \
			--enable-libpath=/usr/lib/bjlib2 \
			--enable-progpath=/usr/bin \
			--datadir=/usr/share \
			LDFLAGS="-L../../com/libs_bin64"
		make
	popd

	pushd rastertocanonij
		./autogen.sh --prefix=/usr \
			--enable-progpath=/usr/bin
		make
	popd

	pushd tocanonij
		./autogen.sh --prefix=/usr
		make
	popd

	pushd tocnpwg
		./autogen.sh --prefix=/usr
		make
	popd
}

check() {

	cd "$pkgname-source-$pkgver-1"

    pushd cmdtocanonij2
    make check
    popd

    pushd cmdtocanonij3 
    make check
    popd

    pushd cnijbe2
    make check
    popd

    pushd lgmon3
    make check
    popd

    pushd rastertocanonij
    make check
    popd

    pushd tocanonij
    make check
    popd

    pushd tocnpwg
    make check
    popd
}

package() {
	mkdir -p "$pkgdir/usr/lib/bjlib2"
    mkdir -p "$pkgdir/usr/bin"
    mkdir -p "$pkgdir/usr/lib/cups/filter"
    mkdir -p "$pkgdir/usr/lib/cups/backend"
    mkdir -p "$pkgdir/usr/share/cups/model"

    cd "$pkgname-source-$pkgver-1"

	install -m644 com/ini/cnnet.ini "$pkgdir/usr/lib/bjlib2"
    install -sm755 com/libs_bin64/*.so.* "$pkgdir/usr/lib"
    install -Dm644 doc/LICENSE-cnijfilter-${pkgver}EN.txt \
              "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

    pushd ppd
    	install -m644 *.ppd "$pkgdir/usr/share/cups/model"
    popd

    pushd cmdtocanonij2
        make DESTDIR="$pkgdir/" install
    popd

    pushd cmdtocanonij3 
        make DESTDIR="$pkgdir/" install
    popd

    pushd cnijbe2
        make DESTDIR="$pkgdir/" install
    popd

    pushd lgmon3
        make DESTDIR="$pkgdir/" install
    popd

    pushd rastertocanonij
        make DESTDIR="$pkgdir/" install
    popd

    pushd tocanonij
        make DESTDIR="$pkgdir/" install
    popd

    pushd tocnpwg
        make DESTDIR="$pkgdir/" install
    popd
}
