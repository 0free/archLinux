
pkgname=zfs-dkms
pkgver=2.1.99.r1257.g482505fd42
pkgrel=1
pkgdesc="Kernel modules for the Zettabyte File System."
arch=('any')
url="https://zfsonlinux.org/"
license=('CDDL')
conflicts=('spl-dkms')
provides=("ZFS-MODULE=${pkgver}" "SPL-MODULE=${pkgver}" 'spl-dkms')
provides+=('zfs')
replaces=('spl-dkms')
makedepends=('git')
source=("git+https://github.com/openzfs/zfs.git")
sha256sums=('SKIP')
b2sums=('SKIP')

pkgver() {

    cd "${srcdir}"/zfs

    git describe --long | sed 's/^zfs-//;s/-rc/rc/;s/\([^-]*-g\)/r\1/;s/-/./g'

}

prepare() {

    cd "${srcdir}"/${pkgname%-dkms} #-${pkgver}

    sed -ri "/AC_CONFIG_FILES/,/]\)/{
/AC_CONFIG_FILES/n
/]\)/n
/^\s*(module\/.*|${pkgname%-dkms}.release|Makefile)/!d
}" configure.ac

    autoreconf -fi

}

build() {

    cd "${srcdir}"/${pkgname%-dkms} #-${pkgver}
    
    ./scripts/dkms.mkconf -n ${pkgname%-dkms} -v ${pkgver} -f dkms.conf

    ./scripts/make_gitrev.sh include/zfs_gitrev.h

}

package() {

    depends=('dkms')

    cd "${srcdir}"/${pkgname%-dkms} #-${pkgver}

    dkmsdir="${pkgdir}/usr/src/${pkgname%-dkms}-${pkgver}"

    install -d "${dkmsdir}"/{config,scripts}

    cp -a configure dkms.conf Makefile.in META ${pkgname%-dkms}_config.h.in ${pkgname%-dkms}.release.in include/ module/ "${dkmsdir}"/

    cp config/compile config/config.* config/missing config/*sh "${dkmsdir}"/config/

    cp scripts/enum-extract.pl scripts/dkms.postbuild "${dkmsdir}"/scripts/

}
