# Maintainer: Allan McRae <allan@archlinux.org>
# Contributor: Tom Newsom <Jeepster@gmx.co.uk>

pkgname=sudo
pkgver=1.7.1
pkgrel=2
pkgdesc="Give certain users the ability to run some commands as root"
arch=('i686' 'x86_64')
url="http://www.sudo.ws/sudo/"
license=('custom')
depends=('glibc' 'pam')
backup=('etc/sudoers' 'etc/pam.d/sudo')
source=(ftp://ftp.sudo.ws/pub/sudo/$pkgname-$pkgver.tar.gz 
        sudo-1.7.1-setenv.diff
        sudo.pam)
options=('!libtool' '!makeflags')
md5sums=('af672524b2c854a67612bf4c743f58b8'
         '010a24456e4dc7b2e904b2e6f18239a2'
         '4e7ad4ec8f2fe6a40e12bcb2c0b256e3')

build() {
  cd $srcdir/$pkgname-$pkgver || return 1
  patch -Np1 -i $srcdir/sudo-1.7.1-setenv.diff || return 1

  ./configure --prefix=/usr --with-pam --libexecdir=/usr/lib \
    --with-env-editor --with-all-insults --with-logfac=auth || return 1
  make || return 1
  make DESTDIR=$pkgdir install || return 1
  install -Dm644 $srcdir/sudo.pam $pkgdir/etc/pam.d/sudo || return 1

  install -Dm644 LICENSE $pkgdir/usr/share/licenses/sudo/LICENSE || return 1
}
