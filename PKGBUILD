# Maintainer: Evangelos Foutras <evangelos@foutrelis.com>
# Contributor: Allan McRae <allan@archlinux.org>
# Contributor: Tom Newsom <Jeepster@gmx.co.uk>

pkgname=sudo
_sudover=1.8.6p1
pkgver=${_sudover/p/.p}
pkgrel=1
pkgdesc="Give certain users the ability to run some commands as root"
arch=('i686' 'x86_64')
url="http://www.sudo.ws/sudo/"
license=('custom')
depends=('glibc' 'pam')
backup=('etc/sudoers' 'etc/pam.d/sudo')
options=('!libtool')
source=(http://www.sudo.ws/sudo/dist/$pkgname-$_sudover.tar.gz
        sudo.pam)
sha256sums=('70d6a3ceb6168c879a80a5fa03cdd06178b95d0ecb6bf87aa0bc7c159da944cc'
            'e7de79d2c73f2b32b20a8e797e54777a2bf19788ec03e48decd6c15cd93718ae')

build() {
  cd "$srcdir/$pkgname-$_sudover"

  ./configure \
    --prefix=/usr \
    --libexecdir=/usr/lib \
    --with-logfac=auth \
    --with-pam \
    --with-env-editor \
    --with-passprompt="[sudo] password for %p: " \
    --with-all-insults
  make
}

check() {
  cd "$srcdir/$pkgname-$_sudover"
  make check
}

package() {
  cd "$srcdir/$pkgname-$_sudover"
  make DESTDIR="$pkgdir" install

  install -Dm644 "$srcdir/sudo.pam" "$pkgdir/etc/pam.d/sudo"

  install -Dm644 doc/LICENSE "$pkgdir/usr/share/licenses/sudo/LICENSE"
}

# vim:set ts=2 sw=2 et:
