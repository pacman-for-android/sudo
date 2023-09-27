# Maintainer: Evangelos Foutras <evangelos@foutrelis.com>
# Contributor: Allan McRae <allan@archlinux.org>
# Contributor: Tom Newsom <Jeepster@gmx.co.uk>

pkgname=sudo
_sudover=1.9.14p3
pkgrel=1
pkgver=${_sudover/p/.p}
pkgdesc="Give certain users the ability to run some commands as root"
arch=('x86_64' 'aarch64')
url="https://www.sudo.ws/sudo/"
license=('custom')
depends=('glibc' 'openssl' 'pam' 'libldap' 'zlib')
backup=('data/etc/pam.d/sudo'
        'data/etc/sudo.conf'
        'data/etc/sudo_logsrvd.conf'
        'data/etc/sudoers')
install=$pkgname.install
source=(https://www.sudo.ws/sudo/dist/$pkgname-$_sudover.tar.gz{,.sig}
        sudo_logsrvd.service
        sudo.pam)
sha256sums=('a08318b1c4bc8582c004d4cd9ae2903abc549e7e46ba815e41fe81d1c0782b62'
            'SKIP'
            '3f39975f6457fe66504b765dd210f38fc57eb12f4637cbc95ed8bddad46a71e7'
            'd1738818070684a5d2c9b26224906aad69a4fea77aabd960fc2675aee2df1fa2')
validpgpkeys=('59D1E9CCBA2B376704FDD35BA9F4C021CEA470FB')

prepare() {
  cd $pkgname-$_sudover
  # patch -Np0 -d scripts < ../sudo-install-sh.patch
  cp /data/usr/share/autoconf/build-aux/install-sh scripts/
  sed -i '1s|.*|#!/data/usr/bin/sh|' scripts/{install-sh,mkinstalldirs}
  sed -i 's|INSTALL = $(SHELL)|INSTALL = |' {logsrvd,lib/util,plugins/*,src}/Makefile.in
  sed -i 's|^log_dir=/var/log$|log_dir=/data/var/log|' configure
}

build() {
  cd $pkgname-$_sudover

  ./configure \
    --prefix=/data/usr \
    --sbindir=/data/usr/bin \
    --libexecdir=/data/usr/lib \
    --sysconfdir=/data/etc \
    --with-sssd-conf=/data/etc/sssd/sssd.conf \
    --with-logpath=/data/var/log/sudo.log \
    --with-nsswitch=/data/etc/nsswitch.conf \
    --with-rundir=/data/var/run/sudo \
    --with-vardir=/data/var/db/sudo \
    --with-iologdir=/data/var/log/sudo-io \
    --with-relaydir=/data/var/log/sudo_logsrvd \
    --with-logfac=auth \
    --disable-tmpfiles.d \
    --with-pam \
    --with-sssd \
    --with-ldap \
    --with-ldap-conf-file=/data/etc/openldap/ldap.conf \
    --with-ldap-secret-file=/data/etc/ldap.secret \
    --with-env-editor \
    --with-passprompt="[sudo] password for %p: " \
    --with-all-insults
  make
}

check() {
  cd $pkgname-$_sudover
  make check
}

package() {
  depends+=('libcrypto.so' 'libssl.so')

  cd $pkgname-$_sudover
  make DESTDIR="$pkgdir" SHELL=/data/usr/bin/sh install

  # sudo_logsrvd service file (taken from sudo-logsrvd-1.9.0-1.el8.x86_64.rpm)
  install -Dm644 -t "$pkgdir/data/usr/lib/systemd/system" ../sudo_logsrvd.service

  # Remove sudoers.dist; not needed since pacman manages updates to sudoers
  rm "$pkgdir/data/etc/sudoers.dist"

  # Remove /run/sudo directory; we create it using systemd-tmpfiles
  # rmdir "$pkgdir/run/sudo"
  # rmdir "$pkgdir/run"

  install -Dm644 "$srcdir/sudo.pam" "$pkgdir/data/etc/pam.d/sudo"

  install -Dm644 LICENSE.md -t "$pkgdir/data/usr/share/licenses/sudo"
}

# vim:set ts=2 sw=2 et:
