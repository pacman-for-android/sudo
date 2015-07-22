# Maintainer: Evangelos Foutras <evangelos@foutrelis.com>
# Contributor: Allan McRae <allan@archlinux.org>
# Contributor: Tom Newsom <Jeepster@gmx.co.uk>

pkgname=sudo
_sudover=1.8.14p2
pkgver=${_sudover/p/.p}
pkgrel=2
pkgdesc="Give certain users the ability to run some commands as root"
arch=('i686' 'x86_64')
url="http://www.sudo.ws/sudo/"
license=('custom')
groups=('base-devel')
depends=('glibc' 'pam' 'libldap')
backup=('etc/sudoers' 'etc/pam.d/sudo')
install=$pkgname.install
source=(http://www.sudo.ws/sudo/dist/$pkgname-$_sudover.tar.gz{,.sig}
        sudo.pam no-tty.patch)
sha256sums=('b4bca9cca52fc6a409709995014af5e9fb905a4a6c5bda977f78e568954dfe21'
            'SKIP'
            'd1738818070684a5d2c9b26224906aad69a4fea77aabd960fc2675aee2df1fa2'
            '5f453de28dcd923d2328bf79bfa6d068a44532fe07e3c85e74cb1f78d74231d9')
validpgpkeys=('CCB24BE9E9481B15D34159535A89DFA27EE470C4')

prepare() {
  cd "$srcdir/$pkgname-$_sudover"
  patch -p1 -i ../no-tty.patch
}

build() {
  cd "$srcdir/$pkgname-$_sudover"

  ./configure \
    --prefix=/usr \
    --sbindir=/usr/bin \
    --libexecdir=/usr/lib \
    --with-rundir=/run/sudo \
    --with-vardir=/var/db/sudo \
    --with-logfac=auth \
    --enable-tmpfiles.d \
    --with-pam \
    --with-sssd \
    --with-ldap \
    --with-ldap-conf-file=/etc/openldap/ldap.conf \
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

  # Remove /run/sudo directory from the package; we create it using tmpfiles.d
  rmdir "$pkgdir/run/sudo"
  rmdir "$pkgdir/run"

  install -Dm644 "$srcdir/sudo.pam" "$pkgdir/etc/pam.d/sudo"

  install -Dm644 doc/LICENSE "$pkgdir/usr/share/licenses/sudo/LICENSE"
}

# vim:set ts=2 sw=2 et:
