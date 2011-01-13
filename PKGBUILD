# Maintainer: Allan McRae <allan@archlinux.org>
# Contributor: Tom Newsom <Jeepster@gmx.co.uk>

pkgname=sudo
_ver=1.7.4p5
pkgver=${_ver/[a-z]/.${_ver//[0-9.]/}}
pkgrel=1
pkgdesc="Give certain users the ability to run some commands as root"
arch=('i686' 'x86_64')
url="http://www.sudo.ws/sudo/"
license=('custom')
depends=('glibc' 'pam')
backup=('etc/sudoers' 'etc/pam.d/sudo')
source=(ftp://ftp.sudo.ws/pub/sudo/$pkgname-$_ver.tar.gz
        sudo.pam)
options=('!libtool' '!makeflags')
md5sums=('4c8105507363371dea89ceb7c92187dd'
         '4e7ad4ec8f2fe6a40e12bcb2c0b256e3')

build() {
  cd $srcdir/$pkgname-$_ver

  ./configure --prefix=/usr --with-pam --libexecdir=/usr/lib \
    --with-env-editor --with-all-insults --with-logfac=auth
  make
}

package() {
  cd $srcdir/$pkgname-$_ver
  install -dm755 $pkgdir/var/lib

  make DESTDIR=$pkgdir install
  install -Dm644 $srcdir/sudo.pam $pkgdir/etc/pam.d/sudo

  install -Dm644 LICENSE $pkgdir/usr/share/licenses/sudo/LICENSE
}
