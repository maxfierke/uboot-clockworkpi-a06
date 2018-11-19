# U-Boot: RockPro64
# Maintainer: Kevin Mihelich <kevin@archlinuxarm.org>

buildarch=8

pkgname=uboot-rockpro64
pkgver=2018.11
pkgrel=4
pkgdesc="U-Boot for RockPro64"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
backup=('boot/boot.txt' 'boot/boot.scr')
makedepends=('swig' 'python2' 'bc' 'git' 'rockchip-tools')
install=${pkgname}.install
_commit_rkbin=e7c2917c19eccc1733b6e47f337b6b686e1d2d9e
source=("ftp://ftp.denx.de/pub/u-boot/u-boot-${pkgver/rc/-rc}.tar.bz2"
        "git+https://github.com/rockchip-linux/rkbin.git#commit=$_commit_rkbin"
        'boot.txt'
        'mkscr'
        'RK3399TRUST.ini'
        'evb-rk3399_defconfig')
sha256sums=('737c93f2ea03fec669e840dbee32bcf6238e6924ff5f20e4f1c472ee24e5d37e'
            'SKIP'
            'c5dd074a307dfb0a8fbc66c6afd2929108cbd39be3a838483a2fb9585940c1c1'
            'a4fc8b6b92bc364d6542670d294aa618a8501fb8729f415cc0a3eed776ef0c8e'
            'ff9c68fdb2e49f10b8f11965573844ab7f1e5cec151c18968083b95477245618'
            'eae1e38c955c33fa33dfd1151eb88d7e5167089b7b137a91ea747e1fd8dafe1b')

prepare() {
  cp evb-rk3399_defconfig configs/
}

build() {
  cd u-boot-${pkgver/rc/-rc}

  unset CLFAGS CXXFLAGS CPPFLAGS LDFLAGS

  make evb-rk3399_defconfig
  echo 'CONFIG_IDENT_STRING=" Arch Linux ARM"' >> .config
  make EXTRAVERSION=-${pkgrel}
}

package() {
  cd u-boot-${pkgver/rc/-rc}

  mkdir -p "${pkgdir}/boot"

  tools/mkimage -n rk3399 -T rksd -d ../rkbin/bin/rk33/rk3399_ddr_933MHz_v1.15.bin "${pkgdir}/boot/idbloader.img"
  cat ../rkbin/bin/rk33/rk3399_miniloader_v1.15.bin >> "${pkgdir}/boot/idbloader.img"

  loaderimage --pack --uboot u-boot-dtb.bin "${pkgdir}/boot/uboot.img" 0x200000

  trust_merger ../RK3399TRUST.ini

  cp u-boot-dtb.bin trust.img "${pkgdir}/boot"

  tools/mkimage -A arm -O linux -T script -C none -n "U-Boot boot script" -d ../boot.txt "${pkgdir}/boot/boot.scr"
  cp ../{boot.txt,mkscr} "${pkgdir}"/boot
}
