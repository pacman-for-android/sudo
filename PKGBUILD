# Maintainer: Allan McRae <allan@archlinux.org>
# Contributor: Tom Newsom <Jeepster@gmx.co.uk>

pkgname=sudo
_ver=1.8.4p1
pkgver=${_ver/[a-z]/.${_ver//[0-9.]/}}
pkgrel=1
pkgdesc="Give certain users the ability to run some commands as root"
arch=('i686' 'x86_64')
url="http://www.sudo.ws/sudo/"
license=('custom')
depends=('glibc' 'pam')
backup=('etc/sudoers' 'etc/pam.d/sudo')
options=('!libtool' '!makeflags')
source=(ftp://ftp.sudo.ws/pub/sudo/$pkgname-$_ver.tar.gz{,.sig}
        sudo.pam)
md5sums=('d7f9bf7675bbc6d75e6f9353a175317c'
         '51bc933d386af08038349dd61aa243c3'
         '4e7ad4ec8f2fe6a40e12bcb2c0b256e3')

build() {
  cd "$srcdir/$pkgname-$_ver"

  ./configure --prefix=/usr --with-pam --libexecdir=/usr/lib \
    --with-env-editor --with-all-insults --with-logfac=auth
  make
}

check() {
  cd "$srcdir/$pkgname-$_ver"
  make check
}

package() {
  cd "$srcdir/$pkgname-$_ver"
  make DESTDIR="$pkgdir" install

  install -Dm644 "$srcdir/sudo.pam" "$pkgdir/etc/pam.d/sudo"

  install -Dm644 doc/LICENSE "$pkgdir/usr/share/licenses/sudo/LICENSE"
}
