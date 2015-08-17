# Maintainer: lilydjwg <lilydjwg@gmail.com>
# Maintainer: Hugo Osvaldo Barrera <hugo@osvaldobarrera.com.ar>
# Contributor: Matthias Maennich <arch@maennich.net>
# Contributor: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>
# Contributor: Jan-Erik Meyer-Luetgens <nyan at meyer-luetgens dot de>

# build command
# sudo extra-x86_64-build -- -I ../shiboken/shiboken-1.2.1-3-x86_64.pkg.tar.xz -I ../python-pyside/python-pyside-common-1.2.1-5-x86_64.pkg.tar.xz -I ../python-pyside/python-pyside-1.2.1-5-x86_64.pkg.tar.xz

pkgname=python-pyside-tools
_pkgrealname=pyside-tools
pkgver=0.2.15
pkgrel=6
pkgdesc="UI Compiler (pyside-uic) plus Qt Resource Compiler (pyside-rcc4) for PySide. For both Python 2 and 3."
arch=('i686' 'x86_64')
license=('LGPL')
url="http://qt-project.org/wiki/PySide"
depends=('qt4')
optdepends=('python-pyside: for PySide with Python 3'
            'python2-pyside: for PySide with Python 2')
replaces=('pyside-tools')
conflicts=('python2-pyside-tools')
provides=("python2-pyside-tools=$pkgver")
makedepends=('cmake' 'automoc4' 'python-pyside' 'shiboken>=1.2.1')
source=("https://github.com/PySide/Tools/archive/$pkgver.tar.gz")
md5sums=('e542b9536bd9d35599ede225c9311cc8')

build(){
    cd $srcdir/Tools-$pkgver
    mkdir -p build && cd build

    _pyverflags=$(python3 -c 'import sysconfig; print(sysconfig.get_config_var("SOABI"))')
    cmake ../ -DCMAKE_INSTALL_PREFIX=/usr \
              -DCMAKE_BUILD_TYPE=Release \
              -DPYTHON_SUFFIX=.${_pyverflags}
    # If you want build with Python 2:
    # _pyver=2.7
    # cmake ../ -DCMAKE_INSTALL_PREFIX=/usr \
    #           -DCMAKE_BUILD_TYPE=Release \
    #           -DSHIBOKEN_PYTHON_SUFFIX=-python$_pyver \
    #           -DPYTHON_EXECUTABLE=/usr/bin/python$_pyver \
    #           -DQT_QMAKE_EXECUTABLE=qmake-qt4 \
    #           -DPYTHON_BASENAME=-python$_pyver

    make
}

package(){
    cd "$srcdir/Tools-$pkgver/build"
    make DESTDIR="$pkgdir" install
    sed -e "s/env python/env python2/g" "${pkgdir}/usr/bin/pyside-uic" > "${pkgdir}/usr/bin/pyside-uic-py2"
    chmod +x "${pkgdir}/usr/bin/pyside-uic-py2"
    mkdir -p "${pkgdir}/usr/lib/python2.7/site-packages"
    _pyver=$(python3 -c 'import sys; print(".".join(str(x) for x in sys.version_info[:2]))')
    ln -s ../../python${_pyver}/site-packages/pysideuic "${pkgdir}/usr/lib/python2.7/site-packages/pysideuic"
}

