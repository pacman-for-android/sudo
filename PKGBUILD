# Maintainer: Evangelos Foutras <evangelos@foutrelis.com>
# Contributor: Allan McRae <allan@archlinux.org>
# Contributor: Tom Newsom <Jeepster@gmx.co.uk>

pkgname=sudo
_sudover=1.8.9p3
pkgver=${_sudover/p/.p}
pkgrel=2
pkgdesc="Give certain users the ability to run some commands as root"
arch=('i686' 'x86_64')
url="http://www.sudo.ws/sudo/"
license=('custom')
groups=('base-devel')
depends=('glibc' 'pam')
backup=('etc/sudoers' 'etc/pam.d/sudo')
source=(http://www.sudo.ws/sudo/dist/$pkgname-$_sudover.tar.gz{,.sig}
        sudo-1.8.9p3-remove-backchannel-event-if-we-get-eof.patch
        sudo.pam)
sha256sums=('a2b1f0ec8aeb929c8430b1514cb53e2c2f882ea26cbb43426883d1cb6d22c5b7'
            'SKIP'
            '79d86c92723519fed1fa3db60ebc66b400435237d82e9bfdc899db4550c259b5'
            'e7de79d2c73f2b32b20a8e797e54777a2bf19788ec03e48decd6c15cd93718ae')

prepare() {
  cd "$srcdir/$pkgname-$_sudover"

  patch -Np1 -i "$srcdir/sudo-1.8.9p3-remove-backchannel-event-if-we-get-eof.patch"
}

build() {
  cd "$srcdir/$pkgname-$_sudover"

  ./configure \
    --prefix=/usr \
    --sbindir=/usr/bin \
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
