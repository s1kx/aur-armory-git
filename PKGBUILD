# Maintainer: Jonathan la Cour <jon@lacour.me>
# Contributor: Pieter Kokx <pieter@kokx.nl>
# Contributor: Patrick Glandien <patrick@synix.io>
pkgname=armory-git
pkgver=v0.96.r0.ga3d01aa7
pkgrel=1
pkgdesc="Full-featured Bitcoin wallet management application"
arch=('i686' 'x86_64')
url="https://github.com/goatpig/BitcoinArmory"
license=('AGPL3')
depends=('crypto++' 'swig' 'qt4' 'python' 'python-twisted' 'python-pyqt4' 'python-bsddb' 'python-psutil')
makedepends=('git' 'gcc' 'make')
optdepends=('bitcoin-core: Communicate with the Bitcoin network')
install="${pkgname}.install"
provides=('armory')
conflicts=('armory')
source=("$pkgname"::'git+https://github.com/goatpig/BitcoinArmory.git'
        'run-armory.sh')
noextract=()
sha256sums=('SKIP'
            '4b8ab285588ec07601fb4d9580b84e11a513635a102d92ee7c283261d0b6c0dc')

pkgver() {
    cd "$srcdir/$pkgname"
    git describe --tags --long $(git rev-list --tags --max-count=1) | sed -E 's/([^-]*-g)/r\1/;s/-/./g'
}

prepare() {
    cd "$srcdir/$pkgname"

    git submodule update --init

    ## Get Python2 Version
    _py2longver=$(pacman -Qi python2 | grep "Version" | sed 's/^Version\s*:\s//')
    _py2ver=${_py2longver%.*}
    PYTHON_VERSION=${_py2ver}  ./autogen.sh
}

build() {
    cd "$srcdir/$pkgname"

    ## Get Python2 Version
    _py2longver=$(pacman -Qi python2 | grep "Version" | sed 's/^Version\s*:\s//')
    _py2ver=${_py2longver%.*}
    PYTHON_VERSION=${_py2ver} ./configure

    ## Build using current python2 version
    PYTHON_VERSION=${_py2ver} make -j"${nproc}"
}

package() {
    install -Dm644 "$srcdir/$pkgname/dpkgfiles/armory.desktop" "$pkgdir/usr/share/applications/armory.desktop"
    install -Dm644 "$srcdir/$pkgname/dpkgfiles/armoryoffline.desktop" "$pkgdir/usr/share/applications/armoryoffline.desktop"
    install -Dm644 "$srcdir/$pkgname/dpkgfiles/armorytestnet.desktop" "$pkgdir/usr/share/applications/armorytestnet.desktop"
    install -Dm644 "$srcdir/$pkgname/img/armory_icon_64x64.png" "$pkgdir/usr/share/armory/img/armory_icon_64x64.png"
    install -Dm644 "$srcdir/$pkgname/img/armory_icon_green_64x64.png" "$pkgdir/usr/share/armory/img/armory_icon_green_64x64.png"

    install -Dm 755 "$srcdir/run-armory.sh" "$pkgdir/usr/bin/armory"

    mkdir -p "$pkgdir/opt"
    cp -R "$srcdir/$pkgname/" "$pkgdir/opt/"

    rm -rf "$pkgdir/opt/$pkgname/cppForSwig/"
    rm -rf "$pkgdir/opt/$pkgname/.git/"
    rm -rf "$pkgdir/opt/$pkgname/.gitignore"
}
