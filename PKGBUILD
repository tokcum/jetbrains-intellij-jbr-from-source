# Maintainer: Tobias Mucke <tobias.mucke@gmail.com>

# Header according to Arch Package Guidelines
pkgname=jetbrains-intellij-jbr-from-source

_major=11
_minor=0
_patch=6
_build=725

pkgver=${_major}.${_minor}.${_patch}b${_build}
pkgrel=1
pkgdesc="JetBrains Runtime (JBR) based on OpenJDK Java ${_major}, built from source"
arch=('x86_64')
url='https://github.com/JetBrains/JetBrainsRuntime'
license=('GPL2')
depends=()
makedepends=('git' 'make' 'sed')
optdepends=()

# Source from github.com isn't defined here because its downloaded by prepare() more efficiently
#source=("git+https://github.com/JetBrains/JetBrainsRuntime.git")
source=("jbr.sh"
        "jbr.csh")
sha256sums=('35f9558a36869bf53fbf1a44b47121844ded69eeb52c3719b7126f06160fa305'
            '982f3cc07254dada1ea2e5f66e9994c48c628f325154e33cabf9dd03680dfc4e')

# Local vars
_branch="jb${_major}_${_minor}_${_patch}-b${_build}"
_repo=("JetBrainsRuntime")
_source=(https://github.com/JetBrains/${_repo}.git)
_prefix="/usr/lib/jvm/java-${_major}-jbr"

# Packaging functions
prepare() {
  git clone --depth 1 --branch ${_branch} ${_source} ${pkgname}-${pkgver}
}

build() {
  sed "s%_prefix%${_prefix}%g" jbr.sh > jbr-built.sh
  sed "s%_prefix%${_prefix}%g" jbr.csh > jbr-built.csh

  cd ${pkgname}-${pkgver}
  ./configure --prefix=${_prefix} --disable-warnings-as-errors 
  make images
}

package() {

  install -dm 755 "${pkgdir}/etc/profile.d"
  install -Dm 644 jbr-built.sh "${pkgdir}/etc/profile.d/jbr.sh"
  install -Dm 644 jbr-built.csh "${pkgdir}/etc/profile.d/jbr.csh"

  cd ${pkgname}-${pkgver}
  make INSTALL_PREFIX="${pkgdir}/" install
}

