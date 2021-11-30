# U-Boot: ClockworkPI A06
# Based on PKGBUILD for pinebookpro: https://gitlab.manjaro.org/manjaro-arm/packages/core/uboot-pinebookpro-bsp
# Maintainer (this port): Max Fierke
# Maintainer (upstream): Dan Johansen <strit@manjaro.org>

pkgname=uboot-clockworkpi-a06
pkgver=2021.10
pkgrel=1
_tfaver=2.6
pkgdesc="U-Boot for ClockworkPI A06"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
provides=('uboot')
conflicts=('uboot')
install=${pkgname}.install
source=("https://ftp.denx.de/pub/u-boot/u-boot-${pkgver/rc/-rc}.tar.bz2"
        "https://git.trustedfirmware.org/TF-A/trusted-firmware-a.git/snapshot/trusted-firmware-a-$_tfaver.tar.gz"
        "0003-uboot-clockworkpi-a06.patch")
sha256sums=('cde723e19262e646f2670d25e5ec4b1b368490de950d4e26275a988c36df0bd4'
            '4e59f02ccb042d5d18c89c849701b96e6cf4b788709564405354b5d313d173f7'
            '8b6d3477be3fc2832775ee6eaa12769e650b05efaac52a22dacfb72cac7c6a04')

#prepare() {
#  # Why doesn't this untar automatically?
#  if [ ! -d "u-boot-${pkgver/rc/-rc}" ]; then
#    tar -xf u-boot-${pkgver/rc/-rc}.tar
#  fi
#
#  cd "${srcdir}/u-boot-${pkgver/rc/-rc}"
#  patch -Np1 -i "${srcdir}/0001-uboot-clockworkpi-a06.patch"
#}

  cd ./u-boot-${pkgver/rc/-rc}
  patch -Np1 -i "${srcdir}/0003-uboot-clockworkpi-a06.patch"
}

  # Build trust.img
  # cd rkbin/
  # tools/trust_merger RKTRUST/RK3399TRUST.ini
  # mv trust.img ../

  # Build u-boot-dtb.bin
  # cd ../u-boot-${pkgver/rc/-rc}
  # unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS
  # make clockworkpi-a06-rk3399_defconfig
  # echo 'CONFIG_IDENT_STRING=" Manjaro ARM"' >> .config
  # make EXTRAVERSION=-${pkgrel} all
  # cd ../

  # Build u-boot.img
  # rkbin/tools/loaderimage --pack --uboot "u-boot-${pkgver/rc/-rc}/u-boot-dtb.bin" "u-boot-${pkgver/rc/-rc}/uboot.img" 0x200000
# }

package() {
  # cd u-boot-${pkgver/rc/-rc}

  mkdir -p "${pkgdir}/boot/extlinux"

  cp uboot.img trust.img idbloader.img u-boot-dtb.bin "${pkgdir}/boot"
}
