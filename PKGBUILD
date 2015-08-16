#Maintainer:	Jesse	Jaara		<gmail.com: jesse.jaara>
#Contributor:	Aaron	Griffin		<archlinux.org: aaron>
#Contributor:	Tobias	Powalowski	<archlinux.org: tpowa>
#Contributor:	Thomas	Bächler		<archlinux.org: thomas>

pkgname=lib32-systemd-tools
pkgver=192
pkgrel=1
arch=('x86_64')
pkgdesc="The userspace dev tools (udev) (32-bit)"
url="http://www.freedesktop.org/wiki/Software/systemd"
license=('GPL')
options=(!libtool)
depends=('lib32-glib2')
makedepends=('gcc-multilib' 'gperf' 'lib32-kmod' 'lib32-util-linux' 'lib32-libcap' 'intltool')
provides=('lib32-udev')
conflicts=('lib32-udev')
source=("http://www.freedesktop.org/software/systemd/systemd-$pkgver.tar.xz"
        '0001-Reinstate-TIMEOUT-handling.patch'
	'use-split-usr-path.patch'
	'gudev-1.0.pc'
	'libudev.pc')

build() {
  export CC="gcc -m32"
  export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"

  cd "${srcdir}/systemd-${pkgver}"

  patch -p1 -i ../0001-Reinstate-TIMEOUT-handling.patch
#  patch -p1 -i ../use-split-usr-path.patch

  ./configure \
      --libexecdir=/usr/lib32 \
      --libdir=/usr/lib32 \
      --localstatedir=/var \
      --sysconfdir=/etc \
      --enable-split-usr \
      --disable-ima \
      --disable-selinux \
      --disable-xz \
      --disable-tcpwrap \
      --disable-pam \
      --disable-acl \
      --disable-audit \
      --disable-libcryptsetup \
      --disable-gcrypt \
      --disable-qrencode \
      --disable-binfmt \
      --disable-vconsole \
      --disable-readahead \
      --disable-quotacheck \
      --disable-randomseed \
      --disable-logind \
      --disable-hostnamed \
      --disable-timedated \
      --disable-localed \
      --disable-coredump \
      --disable-keymap \
      --disable-manpages \
      --with-distro=arch \
      --with-usb-ids-path=/usr/share/hwdata/usb.ids \
      --with-pci-ids-path=/usr/share/hwdata/pci.ids \
      --with-firmware-path=/usr/lib/firmware/updates:/lib/firmware/updates:/usr/lib/firmware:/lib/firmware

  make libudev.la src/gudev/gudevmarshal.h libgudev-1.0.la
}

package() {
  cd "${srcdir}/systemd-${pkgver}"

  mkdir -p "${pkgdir}/usr/lib32/pkgconfig"

  cp .libs/libgudev-1.0.a .libs/libgudev-1.0.so.0.1.2 "${pkgdir}/usr/lib32/"
  cp .libs/libudev.a .libs/libudev.so.1.1.3 "${pkgdir}/usr/lib32/"

  ln -s libgudev-1.0.so.0.1.2 "${pkgdir}/usr/lib32/libgudev-1.0.so.0"
  ln -s libgudev-1.0.so.0.1.2 "${pkgdir}/usr/lib32/libgudev-1.0.so"

  ln -s libudev.so.1.1.3 "${pkgdir}/usr/lib32/libudev.so.1"
  ln -s libudev.so.1.1.3 "${pkgdir}/usr/lib32/libudev.so"

  cp "${srcdir}/libudev.pc" "${pkgdir}/usr/lib32/pkgconfig/"
  cp "${srcdir}/gudev-1.0.pc" "${pkgdir}/usr/lib32/pkgconfig/"
}

md5sums=('e8692055923e87f7f9cb634d44314edb'
         'd9827d244813e1cf29e17f6e6626db30'
         'fd9501032828ac40816d79e743eacfea'
         '63282828185c5edf875ac1c3d34b50f8'
         '9ea15eca9b3b452e361a11dec5aca440')
