# U-Boot: RockPro64 based on PKGBUILD for Rock64
# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Kevin Mihelich 
# Contributor: Adam <adam900710@gmail.com>

pkgname=uboot-rockpro64
pkgver=2020.04
pkgrel=1
_tfaver=2.3
pkgdesc="U-Boot for RockPro64"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
backup=('boot/extlinux/extlinux.conf')
makedepends=('git' 'arm-none-eabi-gcc' 'dtc' 'bc')
install=${pkgname}.install
source=("ftp://ftp.denx.de/pub/u-boot/u-boot-${pkgver/rc/-rc}.tar.bz2"
        "https://git.trustedfirmware.org/TF-A/trusted-firmware-a.git/snapshot/trusted-firmware-a-$_tfaver.tar.gz"
        'extlinux.conf')
sha256sums=('fe732aaf037d9cc3c0909bad8362af366ae964bbdac6913a34081ff4ad565372'
            '37f917922bcef181164908c470a2f941006791c0113d738c498d39d95d543b21'
            'c99eb08d881056f15027d260909cb4c8a11b63fe3d272fe754a8b01d92601afa')

build() {
  cd trusted-firmware-a-$_tfaver
  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS
  make PLAT=rk3399
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
