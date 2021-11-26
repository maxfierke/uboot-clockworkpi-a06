# U-Boot: ClockworkPI A06 based on PKGBUILD for RockPro64
# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Kevin Mihelich
# Contributor: Adam <adam900710@gmail.com>

pkgname=uboot-clockworkpi-a06
pkgver=2021.10
pkgrel=1
_tfaver=2.5
pkgdesc="U-Boot for ClockworkPI A06"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
makedepends=('git' 'arm-none-eabi-gcc' 'dtc' 'bc')
provides=('uboot')
conflicts=('uboot')
install=${pkgname}.install
source=("https://ftp.denx.de/pub/u-boot/u-boot-${pkgver/rc/-rc}.tar.bz2"
        "https://git.trustedfirmware.org/TF-A/trusted-firmware-a.git/snapshot/trusted-firmware-a-$_tfaver.tar.gz"
        "0001-fix-rk3399-suspend-correct-LPDDR4-resume-sequence.patch"
        "0002-fix-rockchip-rk3399-fix-dram-section-placement.patch"
        "0003-uboot-clockworkpi-a06.patch")
sha256sums=('cde723e19262e646f2670d25e5ec4b1b368490de950d4e26275a988c36df0bd4'
            'ad8a2ffcbcd12d919723da07630fc0840c3c2fba7656d1462e45488e42995d7c'
            '2cd375c5456e914208eb1b36adb4e78ee529bdd847958fb518a9a1be5b078b12'
            '52c3641b59422cb4174ac5c0c1d8617917ac05472d0d0a3437db128c077673fb'
            '4f53d757c3fbb0436b7936a4ee18ce001637058e3fc9be6e7b9e73b6811e6d8d')

prepare() {
  # Why doesn't this untar automatically?
  if [ ! -d "u-boot-${pkgver/rc/-rc}" ]; then
    tar -xf u-boot-${pkgver/rc/-rc}.tar
  fi

  cd trusted-firmware-a-$_tfaver
  patch -Np1 -i "${srcdir}/0001-fix-rk3399-suspend-correct-LPDDR4-resume-sequence.patch"    #Suspend
  patch -Np1 -i "${srcdir}/0002-fix-rockchip-rk3399-fix-dram-section-placement.patch"       #GCC 11 fix
  cd ../u-boot-${pkgver/rc/-rc}
  patch -Np1 -i "${srcdir}/0003-uboot-clockworkpi-a06.patch"
}

build() {
  cd trusted-firmware-a-$_tfaver
  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS
  make PLAT=rk3399
  cp build/rk3399/release/bl31/bl31.elf ../u-boot-${pkgver/rc/-rc}
  cd ../u-boot-${pkgver/rc/-rc}
  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS
  make clockworkpi-a06-rk3399_defconfig
  echo 'CONFIG_IDENT_STRING=" Manjaro ARM"' >> .config
  make EXTRAVERSION=-${pkgrel} all u-boot.itb
}

package() {
  cd u-boot-${pkgver/rc/-rc}

  mkdir -p "${pkgdir}/boot/extlinux"

  cp u-boot.itb idbloader.img "${pkgdir}/boot"
}
