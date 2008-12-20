# Maintainer: Allan McRae <allan@archlinux.org>
# Contributor: Tom Newsom <Jeepster@gmx.co.uk>

pkgname=sudo
pkgver=1.7.0
pkgrel=1
pkgdesc="Give certain users the ability to run some commands as root"
arch=('i686' 'x86_64')
url="http://www.sudo.ws/sudo/"
license=('custom')
depends=('glibc' 'pam')
backup=('etc/sudoers' 'etc/pam.d/sudo')
source=(ftp://ftp.sudo.ws/pub/sudo/$pkgname-$pkgver.tar.gz sudo.pam)
options=('!libtool' '!makeflags')
md5sums=('5fd96bba35fe29b464f7aa6ad255f0a6'
         '4e7ad4ec8f2fe6a40e12bcb2c0b256e3')

build() {
  cd $srcdir/$pkgname-$pkgver || return 1

  ./configure --prefix=/usr --with-pam --libexecdir=/usr/lib \
    --with-env-editor --with-all-insults --with-logfac=auth || return 1
  make || return 1
  make DESTDIR=$pkgdir install || return 1
  install -Dm644 $srcdir/sudo.pam $pkgdir/etc/pam.d/sudo || return 1

  install -Dm644 LICENSE $pkgdir/usr/share/licenses/sudo/LICENSE || return 1
}
