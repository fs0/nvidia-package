# $Id: PKGBUILD 206477 2014-02-27 14:01:31Z svenstaro $
# Maintainer : Thomas Baechler <thomas@archlinux.org>

pkgname=nvidia-custom
pkgver=331.49
_extramodules=extramodules-3.14
pkgrel=1
pkgdesc="NVIDIA drivers for linux"
arch=('i686' 'x86_64')
url="http://www.nvidia.com/"
depends=("nvidia-libgl" "nvidia-utils=${pkgver}")
makedepends=()
conflicts=('nvidia-96xx' 'nvidia-173xx')
license=('custom')
install=nvidia.install
options=(!strip)
source=("nvidia-linux-3.14.patch"
        "ftp://download.nvidia.com/XFree86/Linux-x86/${pkgver}/NVIDIA-Linux-x86-${pkgver}.run"
        "ftp://download.nvidia.com/XFree86/Linux-x86_64/${pkgver}/NVIDIA-Linux-x86_64-${pkgver}-no-compat32.run")
md5sums=('3f23965be76bb91105916441f9cd92d3'
         '176c77f9a10eaa652860dbc24042bff2'
         'd0c944466a36cae8b1e23330b27a721c')

[[ "$CARCH" = "i686" ]] && _pkg="NVIDIA-Linux-x86-${pkgver}"
[[ "$CARCH" = "x86_64" ]] && _pkg="NVIDIA-Linux-x86_64-${pkgver}-no-compat32"

prepare() {
    cd "${srcdir}"
    sh "${_pkg}.run" --extract-only
    cd "${_pkg}"
    # patches here
    patch -p1 < ../nvidia-linux-3.14.patch
}

build() {
    _kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
    cd "${srcdir}"/"${_pkg}"/kernel
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
