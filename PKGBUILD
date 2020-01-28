# Based on the file created for Arch Linux by:
# Contributor: Maxime Gauduin <alucryd@gmail.com>
# Contributor: mortzu <me@mortzu.de>
# Contributor: fnord0 <fnord0@riseup.net>

# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Contributor: Helmut Stult <helmut[at]manjaro[dot]org>

_linuxprefix=linux55-CLEARER
_extramodules=extramodules-5.5-CLEARER
pkgname=$_linuxprefix-acpi_call
_pkgname=acpi_call
pkgver=1.1.0
pkgrel=18
_CLEARERrel=12
pkgdesc='A linux kernel module that enables calls to ACPI methods through /proc/acpi/call'
arch=('i686' 'x86_64')
url="http://github.com/mkottman/acpi_call"
license=('GPL')
depends=("$_linuxprefix")
makedepends=("$_linuxprefix-headers")
provides=("$_pkgname")
groups=("$_linuxprefix-extramodules")
install=acpi_call-CLEARER.install
source=("${url}/archive/v${pkgver}.tar.gz")
sha256sums=('d0d14b42944282724fca76f57d598eed794ef97448f387d1c489d85ad813f2f0')

prepare() {
  sed -i -e 's|acpi/acpi.h|linux/acpi.h|g' acpi_call-${pkgver}/acpi_call.c
  sed -i -e 's|asm/uaccess.h|linux/uaccess.h|g' acpi_call-${pkgver}/acpi_call.c
}

build() {
  _kernver="$(cat /usr/lib/modules/${_extramodules}/version)"

  cd "${_pkgname}-${pkgver}"

  make KVERSION="${_kernver}"
}

package() {
  cd "${_pkgname}-${pkgver}"

  install -dm 755 "${pkgdir}"/usr/lib/{modules/${_extramodules},modules-load.d}
  install -m 644 acpi_call.ko "${pkgdir}"/usr/lib/modules/${_extramodules}/
  gzip "${pkgdir}"/usr/lib/modules/${_extramodules}/acpi_call.ko
  echo acpi_call > "${pkgdir}"/usr/lib/modules-load.d/${pkgname}.conf

  install -dm 755 "${pkgdir}"/usr/share/${pkgname}
  cp -dr --no-preserve='ownership' {examples,support} "${pkgdir}"/usr/share/${pkgname}/

  sed -i "s/EXTRAMODULES=.*/EXTRAMODULES=$_extramodules/" \
    "$startdir/acpi_call-CLEARER.install"
}

# vim: ts=2 sw=2 et:
