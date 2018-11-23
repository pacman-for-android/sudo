# Maintainer: Evangelos Foutras <evangelos@foutrelis.com>
# Contributor: Allan McRae <allan@archlinux.org>
# Contributor: Tom Newsom <Jeepster@gmx.co.uk>

pkgname=sudo
_sudover=1.8.26
pkgver=${_sudover/p/.p}
pkgrel=2
pkgdesc="Give certain users the ability to run some commands as root"
arch=('x86_64')
url="https://www.sudo.ws/sudo/"
license=('custom')
groups=('base-devel')
depends=('glibc' 'libgcrypt' 'pam' 'libldap')
backup=('etc/sudoers' 'etc/pam.d/sudo')
install=$pkgname.install
source=(https://www.sudo.ws/sudo/dist/$pkgname-$_sudover.tar.gz{,.sig}
        bug861-update-fixmdoc-for-bsd-changes.patch
        sudo.pam)
sha256sums=('40da219a6f0341ccb22d04a98988e27f09b831d2561b14c6154067a49ef3fee2'
            'SKIP'
            '54c6c6667d7de4ec02d5e93e6d859d1069a3b76a0eeb4a6bea5d45472ce8ee26'
            'd1738818070684a5d2c9b26224906aad69a4fea77aabd960fc2675aee2df1fa2')
validpgpkeys=('59D1E9CCBA2B376704FDD35BA9F4C021CEA470FB')

prepare() {
  cd "$srcdir/$pkgname-$_sudover"

  # https://bugzilla.sudo.ws/show_bug.cgi?id=861
  patch -Np1 -i ../bug861-update-fixmdoc-for-bsd-changes.patch
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
    --enable-gcrypt \
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

  # Remove sudoers.dist; not needed since pacman manages updates to sudoers
  rm "$pkgdir/etc/sudoers.dist"

  # Remove /run/sudo directory; we create it using systemd-tmpfiles
  rmdir "$pkgdir/run/sudo"
  rmdir "$pkgdir/run"

  install -Dm644 "$srcdir/sudo.pam" "$pkgdir/etc/pam.d/sudo"

  install -Dm644 doc/LICENSE "$pkgdir/usr/share/licenses/sudo/LICENSE"
}

# vim:set ts=2 sw=2 et:
