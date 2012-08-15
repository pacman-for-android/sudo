# Maintainer: Allan McRae <allan@archlinux.org>
# Contributor: Tom Newsom <Jeepster@gmx.co.uk>

pkgname=sudo
_ver=1.8.5p3
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
md5sums=('aa50e0a9ca02ac35d1020881bd3a221f'
         'aceea97d5f4fe063d6803bead339364d'
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
