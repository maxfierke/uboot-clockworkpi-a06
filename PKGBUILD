# U-Boot: RockPro64 based on PKGBUILD for Rock64
# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Kevin Mihelich 
# Contributor: Adam <adam900710@gmail.com>

pkgname=uboot-rockpro64
pkgver=2021.07
pkgrel=3
_tfaver=2.5
pkgdesc="U-Boot for RockPro64"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
backup=('boot/extlinux/extlinux.conf')
makedepends=('git' 'arm-none-eabi-gcc' 'dtc' 'bc')
provides=('uboot')
conflicts=('uboot')
install=${pkgname}.install
source=("ftp://ftp.denx.de/pub/u-boot/u-boot-${pkgver/rc/-rc}.tar.bz2"
        "https://git.trustedfirmware.org/TF-A/trusted-firmware-a.git/snapshot/trusted-firmware-a-$_tfaver.tar.gz"
        "0001-fix-rk3399-suspend-correct-LPDDR4-resume-sequence.patch"
        "0002-fix-rockchip-rk3399-fix-dram-section-placement.patch")
sha256sums=('312b7eeae44581d1362c3a3f02c28d806647756c82ba8c72241c7cdbe68ba77e'
            'ad8a2ffcbcd12d919723da07630fc0840c3c2fba7656d1462e45488e42995d7c'
            '2cd375c5456e914208eb1b36adb4e78ee529bdd847958fb518a9a1be5b078b12'
            '52c3641b59422cb4174ac5c0c1d8617917ac05472d0d0a3437db128c077673fb')

prepare() {
  cd trusted-firmware-a-$_tfaver
  patch -Np1 -i "${srcdir}/0001-fix-rk3399-suspend-correct-LPDDR4-resume-sequence.patch"    #Suspend
  patch -Np1 -i "${srcdir}/0002-fix-rockchip-rk3399-fix-dram-section-placement.patch"       #GCC 11 fix
}

build() {
  cd trusted-firmware-a-$_tfaver
  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS
  make PLAT=rk3399
  cp build/rk3399/release/bl31/bl31.elf ../u-boot-${pkgver/rc/-rc}
  cd ../u-boot-${pkgver/rc/-rc}
  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS
  make rockpro64-rk3399_defconfig
  echo 'CONFIG_IDENT_STRING=" Manjaro ARM"' >> .config
  echo 'CONFIG_USE_PREBOOT=n' >> .config #disables usb boot, but should make it boot every time
  make EXTRAVERSION=-${pkgrel} all u-boot.itb
}

package() {
  cd u-boot-${pkgver/rc/-rc}

  mkdir -p "${pkgdir}/boot/extlinux"
  
  cp u-boot.itb idbloader.img "${pkgdir}/boot"
}
