# Maintainer: Jan Houben <jan@nexttrex.de>
# Contributor: Jesus Alvarez <jeezusjr at gmail dot com>
#
# This PKGBUILD was generated by the archzfs build scripts located at
#
# http://github.com/archzfs/archzfs
#
# ! WARNING !
#
# The archzfs packages are kernel modules, so these PKGBUILDS will only work with the kernel package they target. In this
# case, the archzfs-linux packages will only work with the default linux package! To have a single PKGBUILD target many
# kernels would make for a cluttered PKGBUILD!
#
# If you have a custom kernel, you will need to change things in the PKGBUILDS. If you would like to have AUR or archzfs repo
# packages for your favorite kernel package built using the archzfs build tools, submit a request in the Issue tracker on the
# archzfs github page.
#
pkgbase="zfs-linux-git"
pkgname=("zfs-linux-git" "zfs-linux-git-headers")
_commit='b181b2e604de3f36feab1092c702cdec5e78c693'
_zfsver="2024.04.12.r9105.gb181b2e604"
_kernelver="6.8.5.arch1-1"
_kernelver_full="6.8.5.arch1-1"
_extramodules="${_kernelver_full/.arch/-arch}"

pkgver="${_zfsver}_$(echo ${_kernelver} | sed s/-/./g)"
pkgrel=1
makedepends=("linux-headers=${_kernelver}" "git")
arch=("x86_64")
url="https://openzfs.org/"
source=("git+https://github.com/openzfs/zfs.git#commit=${_commit}" "enforce-kernel-max-version.patch" "linux-6.8-compat.patch" "kernel-6.8-meta.patch")
sha256sums=("SKIP" "c5a9f546638c706844d5aff99f40366db1684679c3318d3a4093e0746748a711" "b875c877069a4c75c7b2b4b22d048e66f415b86f862ef6b3b83d3524694cc973" "1bc3b2e79e481b1bf41e78f9d142de8e97326288ecdc97f8db65496b7c4fd63b")
license=("CDDL")
depends=("kmod" "zfs-utils-git=${_zfsver}" "linux=${_kernelver}")

build() {
    cd "${srcdir}/zfs"
    ./autogen.sh
    ./configure --prefix=/usr --sysconfdir=/etc --sbindir=/usr/bin --libdir=/usr/lib \
                --datadir=/usr/share --includedir=/usr/include --with-udevdir=/usr/lib/udev \
                --libexecdir=/usr/lib --with-config=kernel \
                --with-linux=/usr/lib/modules/${_extramodules}/build \
                --with-linux-obj=/usr/lib/modules/${_extramodules}/build
    make
}

package_zfs-linux-git() {
    pkgdesc="Kernel modules for the Zettabyte File System."
    install=zfs.install
    provides=("zfs" "spl")
    groups=("archzfs-linux-git")
    conflicts=("zfs-dkms" "zfs-dkms-git" "zfs-dkms-rc" "spl-dkms" "spl-dkms-git" 'zfs-linux' 'spl-linux-git' 'spl-linux')
    replaces=("spl-linux-git")
    cd "${srcdir}/zfs"
    make DESTDIR="${pkgdir}" INSTALL_MOD_PATH=${pkgdir}/usr INSTALL_MOD_STRIP=1 install
    # Remove src dir
    rm -r "${pkgdir}"/usr/src
}

package_zfs-linux-git-headers() {
    pkgdesc="Kernel headers for the Zettabyte File System."
    provides=("zfs-headers" "spl-headers")
    conflicts=("zfs-headers" "zfs-dkms" "zfs-dkms-git" "zfs-dkms-rc" "spl-dkms" "spl-dkms-git" "spl-headers")
    cd "${srcdir}/zfs"
    make DESTDIR="${pkgdir}" install
    rm -r "${pkgdir}/lib"
    # Remove reference to ${srcdir}
    sed -i "s+${srcdir}++" ${pkgdir}/usr/src/zfs-*/${_extramodules}/Module.symvers
}
