# U-Boot: ClockworkPI A06 based on PKGBUILD for RockPro64
# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Max Fierke <max@maxfierke.com>
# Contributor: Michael Gollnick
# Contributor: Kevin Mihelich
# Contributor: Adam <adam900710@gmail.com>

pkgname=uboot-clockworkpi-a06
pkgver=2021.10
pkgrel=3
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
            '64fe6b045a5379a6562b28884c66523b45cc51c1cc093a50a0b98ec5c091769b')

prepare() {
  cd u-boot-${pkgver/rc/-rc}
  patch -Np1 -i "${srcdir}/0001-uboot-clockworkpi-a06.patch"
}

build() {
  # Avoid build warnings by editing a .config option in place instead of
  # appending an option to .config, if an option is already present
  update_config() {
    if ! grep -q "^$1=$2$" .config; then
      if grep -q "^# $1 is not set$" .config; then
        sed -i -e "s/^# $1 is not set$/$1=$2/g" .config
      elif grep -q "^$1=" .config; then
        sed -i -e "s/^$1=.*/$1=$2/g" .config
      else
        echo "$1=$2" >> .config
      fi
    fi
  }

  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS

  cd trusted-firmware-a-$_tfaver

  echo -e "\nBuilding TF-A for ClockworkPI A06...\n"
  make PLAT=rk3399
  cp build/rk3399/release/bl31/bl31.elf ../u-boot-${pkgver/rc/-rc}

  cd ../u-boot-${pkgver/rc/-rc}

  echo -e "\nBuilding U-Boot for ClockworkPI A06...\n"
  make clockworkpi-a06-rk3399_defconfig

  update_config 'CONFIG_IDENT_STRING' '" Manjaro Linux ARM"'
  update_config 'CONFIG_OF_LIBFDT_OVERLAY' 'y'
  make EXTRAVERSION=-${pkgrel}
}

package() {
  cd u-boot-${pkgver/rc/-rc}

  mkdir -p "${pkgdir}/boot/extlinux"

  install -D -m 0644 idbloader.img u-boot.itb -t "${pkgdir}/boot"
}
