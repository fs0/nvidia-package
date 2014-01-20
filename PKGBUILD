# $Id: PKGBUILD 203650 2014-01-13 17:13:38Z andyrtr $
# Maintainer : Thomas Baechler <thomas@archlinux.org>

pkgname=nvidia-custom
pkgver=331.38
_extramodules=extramodules-3.13
pkgrel=1
pkgdesc="custom NVIDIA drivers for linux"
arch=('i686' 'x86_64')
url="http://www.nvidia.com/"
depends=("nvidia-libgl" "nvidia-utils")
makedepends=()
conflicts=('nvidia-96xx' 'nvidia-173xx')
license=('custom')
install=nvidia.install
options=(!strip)
source=('nvidia-linux-3.13.patch')
#md5sums=('d267069bc456de269424b4e1b46a3745')

if [ "$CARCH" = "i686" ]; then
    _arch='x86'
    _pkg="NVIDIA-Linux-${_arch}-${pkgver}"
    source+=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
    md5sums+=('16aa229f7f118c8cafad6fb3f4ac082e')
elif [ "$CARCH" = "x86_64" ]; then
    _arch='x86_64'
   _pkg="NVIDIA-Linux-${_arch}-${pkgver}-no-compat32"
    source+=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
    md5sums+=('f2059ae373665cb6c8fb826e1173b04d')
fi

prepare() {
    cd "${srcdir}"
    sh "${_pkg}.run" --extract-only
    cd "${_pkg}"
}

build() {
    _kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
    cd "${srcdir}"/"${_pkg}"/kernel
    patch -p2 < ../../nvidia-linux-3.13.patch
    make SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
    install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia.ko" \
        "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
    install -d -m755 "${pkgdir}/usr/lib/modprobe.d"
    #echo "blacklist nouveau" >> "${pkgdir}/usr/lib/modprobe.d/nvidia.conf"
    sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='${_extramodules}'/" "${startdir}/nvidia.install"
    gzip "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
}
md5sums=('f38f99f72fc34f978be01ac31ad95577'
         'f2059ae373665cb6c8fb826e1173b04d')
