# U-Boot: Rock64
# Maintainer: Kevin Mihelich <kevin@archlinuxarm.org>

buildarch=8

pkgname=uboot-rockpro64
pkgver=2018.11
pkgrel=1
pkgdesc="U-Boot for RockPro64"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
backup=('boot/boot.txt' 'boot/boot.scr')
makedepends=('bc' 'git' 'rockchip-tools' 'swig' 'python2')
install=${pkgname}.install
_commit_rkbin=af17d09dee19b41f4f01e1722eaf6911fb296245
source=("ftp://ftp.denx.de/pub/u-boot/u-boot-${pkgver/rc/-rc}.tar.bz2"
        "git+https://github.com/ayufan-rock64/rkbin.git#commit=$_commit_rkbin"
        'rk3399trust.ini'
        'boot.txt'
        'mkscr')
md5sums=('80604e79f64517afadccb02f3c55a6cb'
         'SKIP'
         'eb9857435ee6533317176c41e008c0cc'
         'b2ef68941641943687da6e4698e9184f'
         '021623a04afd29ac3f368977140cfbfd')

build() {
  cd u-boot-${pkgver/rc/-rc}
  sed -i s/"export MAKE LEX YACC AWK PERL PYTHON PYTHON2 PYTHON3"/"export MAKE LEX YACC AWK PERL PYTHON2 PYTHON3"/ Makefile
  unset CLFAGS CXXFLAGS CPPFLAGS LDFLAGS

  make evb-rk3399_defconfig
  echo 'CONFIG_IDENT_STRING=" Arch Linux ARM"' >> .config
  make EXTRAVERSION=-${pkgrel}
}

package() {
  cd u-boot-${pkgver/rc/-rc}

  mkdir -p "${pkgdir}/boot"

  tools/mkimage -n rk3399 -T rksd -d ../rkbin/rk33/rk3399_ddr_933MHz_v1.13.bin "${pkgdir}/boot/idbloader.img"
  cat ../rkbin/rk33/rk3399_miniloader_v1.12.bin >> "${pkgdir}/boot/idbloader.img"

  loaderimage --pack --uboot u-boot-dtb.bin "${pkgdir}/boot/uboot.img" 0x200000

  trust_merger ../rk3399trust.ini

  cp u-boot-dtb.bin trust.img "${pkgdir}/boot"

  tools/mkimage -A arm -O linux -T script -C none -n "U-Boot boot script" -d ../boot.txt "${pkgdir}/boot/boot.scr"
  cp ../{boot.txt,mkscr} "${pkgdir}"/boot
}
