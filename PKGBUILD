# U-Boot: ClockworkPI A06
# Based on PKGBUILD for pinebookpro: https://gitlab.manjaro.org/manjaro-arm/packages/core/uboot-pinebookpro-bsp
# Maintainer (this port): Max Fierke
# Maintainer (upstream): Dan Johansen <strit@manjaro.org>

pkgname=uboot-clockworkpi-a06
pkgver=2021.10
pkgrel=1
pkgdesc="U-Boot for ClockworkPI A06 (rockchip blobs, upstream u-boot)"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
provides=('uboot')
conflicts=('uboot')
install=${pkgname}.install
# TODO: do this all via qemu-x86_64 on aarch64
# makedepends=('git' 'arm-none-eabi-gcc' 'dtc' 'bc')
# source=("https://ftp.denx.de/pub/u-boot/u-boot-${pkgver/rc/-rc}.tar.bz2"
#         "rkbin::git+https://github.com/rockchip-linux/rkbin.git#commit=7d631e0d5b2d373b54d4533580d08fb9bd2eaad4"
#         "0001-uboot-clockworkpi-a06.patch")
# sha256sums=('cde723e19262e646f2670d25e5ec4b1b368490de950d4e26275a988c36df0bd4'
#             'SKIP'
#             'b82c97b89bc667bdd36327c834543bbf83dcdbf5aac016358a2e7c6cee2a1e2e')
makedepends=()
source=("idbloader.img"
        "uboot.img"
        "trust.img"
        "u-boot-dtb.bin"
        "boot.scr"
        "boot.txt"
        )
sha256sums=("966698f160623f1d469bad96410c1a645614176a6ea341092ca696cf47a49b72"
            "8dabad0aa8356cd5617a5c8e33efa47517fb483296798175ba16ed34f7617583"
            "c82b6b7ea2160c7bdedcd5048b479bb5028434e0811c125fbc2b0080543a6f32"
            "c22c2e894028519a46c313127c265b67678a24e832cfc30fbb884e0f12f283c0"
            "2838632ac8e2484dd222dd1d7ba73b691743a070a3ca3a9ee1d062e4686fee41"
            "3fd2462c877d7a2258132d746a3213c1befe1320fb378bee52670d54fd9dc953"
            )

#prepare() {
#  # Why doesn't this untar automatically?
#  if [ ! -d "u-boot-${pkgver/rc/-rc}" ]; then
#    tar -xf u-boot-${pkgver/rc/-rc}.tar
#  fi
#
#  cd "${srcdir}/u-boot-${pkgver/rc/-rc}"
#  patch -Np1 -i "${srcdir}/0001-uboot-clockworkpi-a06.patch"
#}

# build() {
  # Build idbloader.img
  # cd "${srcdir}"
  # rkbin/tools/mkimage -n rk3399 -T rksd -d rk3399_ddr_800MHz_v1.25.bin idbloader.img
  # cat rkbin/bin/rk33/rk3399_miniloader_v1.26.bin >> idbloader.img

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

  cp uboot.img trust.img idbloader.img u-boot-dtb.bin boot.scr boot.txt "${pkgdir}/boot"
}
