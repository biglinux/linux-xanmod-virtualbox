# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>

# Arch credits:
# Ionut Biru <ibiru@archlinux.org>
# Sébastien Luttringer <seblu@aur.archlinux.org>

_linuxprefix=linux-xanmod
_extramodules=$(find /usr/lib/modules -type d -iname 23.01.03*xanmod* | rev | cut -d "/" -f1 | rev)

pkgname=("$_linuxprefix-virtualbox-host-modules")
pkgver=7.0.6
_pkgver="${pkgver}_OSE"
pkgrel=2301030216
pkgdesc='Virtualbox host kernel modules for Manjaro Kernel'
arch=('x86_64')
url='http://virtualbox.org'
license=('GPL')
groups=("$_linuxprefix-extramodules")
depends=("$_linuxprefix")
makedepends=("$_linuxprefix" "$_linuxprefix-headers" "virtualbox-host-dkms=$pkgver")
provides=('VIRTUALBOX-HOST-MODULES')
conflicts=("$_linuxprefix-virtualbox-modules" 'virtualbox-host-dkms')
replaces=("$_linuxprefix-virtualbox-modules")
install=virtualbox-host-modules.install

build() {
  _kernver="$(cat /usr/lib/modules/$_extramodules/version)"

  # build host modules
  echo 'Host modules'
  fakeroot dkms build --dkmstree "$srcdir" -m vboxhost/${pkgver}_OSE -k ${_kernver}
}

package(){
  _kernver="$(cat /usr/lib/modules/$_extramodules/version)"

  cd "vboxhost/${pkgver}_OSE/$_kernver/$CARCH/module"
  install -Dm644 * -t "$pkgdir/usr/lib/modules/$_extramodules/"

  # compress each module individually
  find "$pkgdir" -name '*.ko' -exec xz -T1 {} +

  # systemd module loading
  printf '%s\n' vboxdrv vboxnetadp vboxnetflt |
  install -Dm644 /dev/stdin "$pkgdir/usr/lib/modules-load.d/$pkgname.conf"
}
