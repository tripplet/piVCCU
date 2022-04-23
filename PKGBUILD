_pkgbase=pivccu
pkgname=pivccu-dkms
pkgver=2022.4.23
pkgrel=1
pkgdesc="pivccu modules"
arch=('x86_64' 'armv7h' 'aarch64')
url="https://github.com/alexreinert/piVCCU/tree/master/kernel"
license=('GPL2')
depends=('dkms')
conflicts=("${_pkgbase}")
sources=('devkey.inc'
         'dkms.conf'
         'dummy_rx8130.c'
         'dw_apb_raw_uart.c'
         'eq3_char_loop.c'
         'fake_hmrf.c'
         'generic_raw_uart.c'
         'generic_raw_uart.h'
         'hb_rf_eth.c'
         'hb_rf_usb.c'
         'hb_rf_usb-2.c'
         'hm.h'
         'led_trigger_timer.c'
         'Makefile'
         'meson_raw_uart.c'
         'pl011_raw_uart.c'
         'plat_eq3ccu2.c'
         'rpi_rf_mod_led.c'
         'rtc-rx8130.c'
         'stack_protector.include')

package() {
  depends=('dkms')
  provides=('PIVCCU-MODULES')

  cd ../kernel  
  install -Dm644 dkms.conf "${pkgdir}"/usr/src/${_pkgbase}-${pkgver}/dkms.conf

  # Set name and version
  sed -e "s/@_PKGBASE@/${_pkgbase}/" \
      -e "s/@PKGVER@/${pkgver}/" \
      -i "${pkgdir}"/usr/src/${_pkgbase}-${pkgver}/dkms.conf

  # Copy sources (including Makefile)
  cp * "${pkgdir}"/usr/src/${_pkgbase}-${pkgver}/
}

