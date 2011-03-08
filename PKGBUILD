# Maintainer: Allan McRae <allan@archlinux.org>
# Contributor: Tom Newsom <Jeepster@gmx.co.uk>

pkgname=sudo
_ver=1.8.0
pkgver=${_ver/[a-z]/.${_ver//[0-9.]/}}
pkgrel=4
pkgdesc="Give certain users the ability to run some commands as root"
arch=('i686' 'x86_64')
url="http://www.sudo.ws/sudo/"
license=('custom')
depends=('glibc' 'pam')
backup=('etc/sudoers' 'etc/pam.d/sudo')
source=(ftp://ftp.sudo.ws/pub/sudo/$pkgname-$_ver.tar.gz
	sudo_l.patch
	sudo_validate_exitval.patch
	sudo_noninteractive.patch
        sudo.pam)
options=('!libtool' '!makeflags')
md5sums=('fa0a35330691af14cb1869f64a65aebc'
         '29656b2f2365e14fa0f8eb94e61f3690'
         '4751aa5557fe43fd8e03e0c7b5affcfc'
         '47d152ade2c9a726684fa1227e46bfe3'
         '4e7ad4ec8f2fe6a40e12bcb2c0b256e3')

build() {
  cd $srcdir/$pkgname-$_ver

  # http://www.sudo.ws/bugs/show_bug.cgi?id=474
  patch -Np1 -i $srcdir/sudo_l.patch
  
  # http://www.sudo.ws/bugs/show_bug.cgi?id=479
  patch -Np1 -i $srcdir/sudo_validate_exitval.patch

  # http://www.sudo.ws/bugs/show_bug.cgi?id=478
  patch -Np1 -i $srcdir/sudo_noninteractive.patch

  ./configure --prefix=/usr --with-pam --libexecdir=/usr/lib \
    --with-env-editor --with-all-insults --with-logfac=auth
  make
}

package() {
  cd $srcdir/$pkgname-$_ver
  install -dm755 $pkgdir/var/lib

  make DESTDIR=$pkgdir install
  install -Dm644 $srcdir/sudo.pam $pkgdir/etc/pam.d/sudo

  install -Dm644 doc/LICENSE $pkgdir/usr/share/licenses/sudo/LICENSE
}
