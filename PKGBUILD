# U-Boot: RockPro64 based on PKGBUILD for Rock64
# Maintainer: Dan Johansen <strit@manjaro.org>
# Contributor: Kevin Mihelich 
# Contributor: Adam <adam900710@gmail.com>

pkgname=uboot-rockpro64
pkgver=2019.10
pkgrel=1
pkgdesc="U-Boot for RockPi 4"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
backup=('boot/boot.txt' 'boot/boot.scr')
makedepends=('bc' 'git' 'rockchip-tools' 'python' 'dtc')
install=${pkgname}.install
_commit_rkbin=0d4740f2c0c897ebc1a074580376689b8454ddd8
source=("https://gitlab.denx.de/u-boot/u-boot/-/archive/v${pkgver}/u-boot-v${pkgver}.tar.bz2"
        "git+https://github.com/rockchip-linux/rkbin.git#commit=$_commit_rkbin"
        'rk3399trust.ini'
        'boot.txt'
        'mkscr')
sha256sums=('6d5908a35a19ac9b7042dbafca49fbd8c4f2d608444ff484448ae4a5287a9d42'
            'SKIP'
            'c83b2423355a6c3e29f8d2ea8aa9ebc1c75b15d635500546ce9dc64faef3f1ce'
            '68723986e0aebdbfea402e7781a263c157ac22022e25eee1a5512e65924df7f2'
            'a4fc8b6b92bc364d6542670d294aa618a8501fb8729f415cc0a3eed776ef0c8e')

build() {
  cd u-boot-v${pkgver}

  unset CLFAGS CXXFLAGS CPPFLAGS LDFLAGS

  make rockpro64-rk3399_defconfig
  sed -i 's/CONFIG_IDENT_STRING=""/CONFIG_IDENT_STRING=" Manjaro ARM"/' .config
  make EXTRAVERSION=-${pkgrel}
}

package() {
  cd u-boot-v${pkgver}

  mkdir -p "${pkgdir}/boot"

  tools/mkimage -n rk3399 -T rksd -d ../rkbin/bin/rk33/rk3399_ddr_800MHz_v1.23.bin "${pkgdir}/boot/idbloader.img"
  cat ../rkbin/bin/rk33/rk3399_miniloader_v1.19.bin >> "${pkgdir}/boot/idbloader.img"

  loaderimage --pack --uboot u-boot-dtb.bin "${pkgdir}/boot/uboot.img" 0x200000

  trust_merger ../rk3399trust.ini

  cp trust.img "${pkgdir}/boot"

  tools/mkimage -A arm -O linux -T script -C none -n "U-Boot boot script" -d ../boot.txt "${pkgdir}/boot/boot.scr"
  cp ../{boot.txt,mkscr} "${pkgdir}"/boot
}
