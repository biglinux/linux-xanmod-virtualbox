# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>

# Arch credits:
# Ionut Biru <ibiru@archlinux.org>
# Sébastien Luttringer <seblu@aur.archlinux.org>

_linuxprefix=linux-xanmod

pkgname=("${_linuxprefix}-virtualbox-host-modules")
pkgver=7.0.14
_pkgver="${pkgver}_OSE"
pkgrel=67101
pkgdesc='Virtualbox host kernel modules for Manjaro Kernel'
arch=('x86_64')
url='http://virtualbox.org'
license=('GPL')
groups=("${_linuxprefix}-extramodules")
depends=("${_linuxprefix}")
makedepends=("${_linuxprefix}-headers" "virtualbox-host-dkms=$pkgver")
provides=('VIRTUALBOX-HOST-MODULES')
conflicts=("${_linuxprefix}-virtualbox-modules" 'virtualbox-host-dkms')
replaces=("${_linuxprefix}-virtualbox-modules")

build() {
  _kernver="$(cat /usr/src/${_linuxprefix}/version)"
  _kernver="$(cat /usr/src/${_linuxprefix}/version)"

  fakeroot dkms build --dkmstree "$srcdir" -m vboxhost/${pkgver}_OSE -k ${_kernver}
}

package() {
  _kernver="$(cat /usr/src/${_linuxprefix}/version)"

  cd "vboxhost/${pkgver}_OSE/${_kernver}/$CARCH/module"
  install -Dm 644 * -t "$pkgdir/usr/lib/modules/${_kernver}/extramodules/"

  # compress each module individually
  find "$pkgdir" -name '*.ko' -exec xz -T1 {} +

  # systemd module loading
  printf '%s\n' vboxdrv vboxnetadp vboxnetflt |
  install -Dm 644 /dev/stdin "$pkgdir/usr/lib/modules-load.d/$pkgname.conf"
}
