# Maintainer: Paul Mattal <paul@archlinux.org>
# Contributor: Tom Newsom <Jeepster@gmx.co.uk>
pkgname=sudo
pkgver=1.6.9p16
pkgrel=1
pkgdesc="Give certain users the ability to run some commands as root"
arch=(i686 x86_64)
url="http://www.sudo.ws/sudo/"
# 2 separate licenses apply, custom and ISC, each covering part of the software
license=('custom' 'ISC')
depends=('glibc' 'pam')
backup=('etc/sudoers' 'etc/pam.d/sudo')
source=(ftp://ftp.sudo.ws/pub/sudo/$pkgname-$pkgver.tar.gz sudo.pam)
md5sums=('b0d01b0c3d55076a47f06f70811be083' '4e7ad4ec8f2fe6a40e12bcb2c0b256e3')
options=('!libtool')

build() {
  cd $startdir/src/$pkgname-$pkgver || return 1

  ./configure --prefix=/usr --with-pam --libexecdir=/usr/lib \
    --with-env-editor --with-all-insults --with-logfac=auth || return 1
  make || return 1
  make DESTDIR=$startdir/pkg install || return 1
  install -D -m644 $startdir/src/sudo.pam $startdir/pkg/etc/pam.d/sudo \
  	|| return 1

  #install the license
  install -D -m644 LICENSE $startdir/pkg/usr/share/licenses/sudo/LICENSE \
  	|| return 1
}
