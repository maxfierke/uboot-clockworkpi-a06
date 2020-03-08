# U-Boot: RockPro64 based on PKGBUILD for Rock64
# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Kevin Mihelich 
# Contributor: Adam <adam900710@gmail.com>

pkgname=uboot-rockpro64
pkgver=2020.01
pkgrel=4
pkgdesc="U-Boot for RockPro64"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
backup=('boot/extlinux/extlinux.conf')
makedepends=('git' 'arm-none-eabi-gcc' 'dtc' 'bc')
install=${pkgname}.install
_commit_atf=22d12c4148c373932a7a81e5d1c59a767e143ac2
source=("ftp://ftp.denx.de/pub/u-boot/u-boot-${pkgver/rc/-rc}.tar.bz2"
        "git+https://github.com/ARM-software/arm-trusted-firmware.git#commit=$_commit_atf"
        'extlinux.conf')
sha256sums=('aa453c603208b1b27bd03525775a7f79b443adec577fdc6e8f06974025a135f1'
            'SKIP'
            'fd1fc35fd56e5e889d0a84843144ddb03119b1e708574606409a2e59ccf74c4e')

build() {
  cd arm-trusted-firmware
  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS
  make realclean
  make PLAT=rk3399 bl31
  cp build/rk3399/release/bl31/bl31.elf ../u-boot-${pkgver/rc/-rc}
  cd ../u-boot-${pkgver/rc/-rc}
  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS
  make rockpro64-rk3399_defconfig
  echo 'CONFIG_IDENT_STRING=" Manjaro ARM"' >> .config
  make EXTRAVERSION=-${pkgrel} all u-boot.itb
}

package() {
  cd u-boot-${pkgver/rc/-rc}

  mkdir -p "${pkgdir}/boot/extlinux"
  
  cp u-boot.itb idbloader.img "${pkgdir}/boot"
  cp "${srcdir}"/extlinux.conf "${pkgdir}"/boot/extlinux/
}
