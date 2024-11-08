# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Andrzej Giniewicz <gginiu@gmail.com>
# Maintainer: Bruno Pagani <archange@archlinux.org>
# Contributor: Martino Pilia <martino.pilia@gmail.com>
# Contributor: Ben Greiner <code-arch@bnavigator.de>

_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=scikit-build
pkgname="${_py}-${_pkg}"
pkgver=0.17.6
pkgrel=3
_pkgdesc=(
  "Improved build system generator"
  "for CPython C, C++, Cython and"
  "Fortran extensions"
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  any
)
url="https://${_pkg}.org"
license=(
  MIT
)
depends=(
  cmake
  "${_py}-distro"
  "${_py}-packaging"
  "${_py}-setuptools"
  "${_py}-wheel"
)
makedepends=(
  git
  "${_py}-build"
  "${_py}-installer"
  "${_py}-hatchling"
  "${_py}-hatch-vcs"
  "${_py}-hatch-fancy-pypi-readme"
)
checkdepends=(
  cython
  gcc
  gcc-fortran
  ninja
  "${_py}-build"
  "${_py}-path"
  "${_py}-pytest"
  "${_py}-pytest-mock"
  "${_py}-requests"
  "${_py}-six"
  "${_py}-virtualenv"
)
_tag=24a78ca5a67f63d28c38e0ca826e746687200947 # git rev-parse ${pkgver}
_http="https://github.com"
_ns="${_pkg}"
_url="${_http}/${_ns}/${_pkg}"
source=(
  "git+${_url}.git#tag=${_tag}?signed"
)
sha256sums=(
  'SKIP'
)
validpgpkeys=(
  # Henry Schreiner <hschrein@cern.ch>
  2FDEC9863E5E14C7BC429F27B9D0E45146A241E8
) 
build() {
  cd \
    "${_pkg}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --no-isolation
}

check() {
  cd \
    "${_pkg}"
  # Disable coverage
  sed \
    -i \
      's|\"--cov\"\, \"--cov-report=xml\"||' \
    noxfile.py
  # Tests need a rw version of site-packages
  "${_py}" \
    -m \
      venv \
    --system-site-packages \
      test-env
  # https://github.com/scikit-build/scikit-build/issues/727
  test-env/bin/python \
    /usr/bin/pytest \
      -vv \
      --color=yes || \
    echo \
      "Tests failed"
}

package() {
  cd \
    "${_pkg}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  install \
    -Dm644 \
    LICENSE \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
}
