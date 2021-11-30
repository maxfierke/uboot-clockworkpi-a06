# U-Boot: ClockworkPI A06 based on PKGBUILD for RockPro64
# Maintainer (this port): Max Fierke <max@maxfierke.com>
# Maintainer (upstream): Dan Johansen <strit@manjaro.org>
# Contributor (this port): Michael Gollnick
# Contributor (upstream): Kevin Mihelich
# Contributor (upstream): Adam <adam900710@gmail.com>

pkgname=uboot-clockworkpi-a06
pkgver=2021.10
pkgrel=1
_tfaver=2.6
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
        "0001-uboot-clockworkpi-a06.patch")
sha256sums=('cde723e19262e646f2670d25e5ec4b1b368490de950d4e26275a988c36df0bd4'
            '4e59f02ccb042d5d18c89c849701b96e6cf4b788709564405354b5d313d173f7'
            'a075aeb700acb08ae61ca152610123e5e51f9bc65cd1f6bb8352aefef6bd1573')

prepare() {
  # Why doesn't this untar automatically?
  if [ ! -d "u-boot-${pkgver/rc/-rc}" ]; then
    tar -xf u-boot-${pkgver/rc/-rc}.tar
  fi

  cd ./u-boot-${pkgver/rc/-rc}
  patch -Np1 -i "${srcdir}/0001-uboot-clockworkpi-a06.patch"
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