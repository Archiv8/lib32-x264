#!/bin/bash

# Created from the original package by

# Disable various shellcheck rules that produce false positives in this file.
# Repository rules should be added to the .shellcheckrc file located in the
# repository root directory, see https://github.com/koalaman/shellcheck/wiki
# and https://archiv8.github.io for further information.
# shellcheck disable=SC2034,SC2154
# [ToDo]: Add files: User documentation
# [ToDo]: Add files: Tooling
# [FixMe]: Namcap warnings and errors

# Maintainer: Alexandre Demers <alexandre.f.demers@gmail.com>
# Contributor: GodronGR <ntheo1979@gmail.com>
# Contributor: Maxime Gauduin <alucryd@archlinux.org>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: damir <damir@archlinux.org>
# Contributor: Paul Mattal <paul@archlinux.org>

_pkgbasename=x264
pkgname=lib32-x264
pkgver=0.164.r3081.19856cc
pkgrel=1
# epoch=3
pkgdesc='Open Source H264/AVC video encoder (32 bit)'
arch=('x86_64')
url='https://www.videolan.org/developers/x264.html'
license=('GPL')
depends=(
#    "x264"
    "x264>=${pkgver}-${pkgrel}" 
    "lib32-glibc" 
    "lib32-l-smash"
    )
makedepends=(
    'git' 
    'nasm' 
    'lib32-gcc-libs'
    )
provides=(
    'lib32-libx264' 
    'libx264.so'
    )
conflicts=(
    'lib32-libx264' 
    'lib32-libx264-10bit' 
    'lib32-libx264-all'
    )
replaces=(
    'lib32-libx264' 
    'lib32-libx264-10bit' 
    'lib32-libx264-all'
    )
_commit='19856cc41ad11e434549fb3cc6a019e645ce1efe'
source=("git+https://git.videolan.org/git/x264.git#commit=${_commit}")
sha256sums=('SKIP')

pkgver() {
    cd x264

    ./version.sh | grep X264_POINTVER | sed -r 's/^#define X264_POINTVER "([0-9]+\.[0-9]+)\.([0-9]+) (.*)"$/\1.r\2.\3/'
}

prepare() {
if [[ -d build ]]; then
    rm -rf build
fi
    mkdir build
}

build() {
    cd build
    export CC="gcc -m32"
    export CXX="g++ -m32"
    export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"

    ../x264/configure \
        --prefix='/usr' \
        --libdir=/usr/lib32 \
        --host=i686-linux-gnu \
        --enable-shared \
        --enable-pic \
        --enable-lto \
        --disable-avs \
        --disable-swscale \
        --disable-lavf
    make
}

package() {
    make -C build DESTDIR="${pkgdir}" install-cli install-lib-shared

    # Keep files in bin since this is not a library only package. 
    # Use the same naming scheme as proposed in Arch's wiki:  https://wiki.archlinux.org/index.php/32-bit_package_guidelines
    # which is "--program-suffix="-32" with Autoconf
    for i in "${pkgdir}/usr/bin/"*; do
        mv "$i" "$i"-32
    done

    rm -rf "${pkgdir}"/usr/include
}
