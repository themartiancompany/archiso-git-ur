# SPDX-License-Identifier: AGPL-3.0

#    -----------------------------------------------------
#    Copyright © 2024, 2025, 2026  Pellegrino Prevete
#
#    All rights reserved
#    -----------------------------------------------------
#
#    This program is free software: you can redistribute
#    it and/or modify it under the terms of the
#    GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of
#    the License, or (at your option) any later version.
#
#    This program is distributed in the hope that it
#    will be useful, but WITHOUT ANY WARRANTY;
#    without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
#    See the GNU Affero General Public License for
#    more details.
#
#    You should have received a copy of the
#    GNU Affero General Public License
#    along with this program.
#    If not, see <https://www.gnu.org/licenses/>.

# Maintainers:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>

_os="$(
  uname \
    -o)"
_git=false
_local=false
_proj="hip"
if [[ ! -v "_ns" ]]; then
  _ns='themartiancompany'
fi
if [[ ! -v "_git_service"  ]]; then
  _git_service="gitlab"
  _git_service="github"
fi

_pkg=archiso
pkgbase="${_pkg}"
pkgname=(
  "${_pkg}-git"
)
pkgver="71".r0.g"f84a6994f092d86ba6b8f45aadbb1f07f4eb6208"
pkgrel=2
_pkgdesc=(
  "Android application utilities."
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'any'
)
_http="https://${_git_service}.com"
_local="${HOME}/${_pkg}"
url="${_http}/${_ns}/${_pkg}"
_gh_api="https://api.${_gh}/repos/${_ns}/${_pkgname}"
license=(
  "AGPL3"
)
depends=(
  'arch-install-scripts'
  'bash'
  'dosfstools'
  'e2fsprogs'
  'erofs-utils'
  'key-gen'
  'libarchive'
  'libcrash-bash'
  'libisoburn'
  'mtools'
)
if [[ "${_os}" == "GNU/Linux" ]] && \
   [[ "${_os}" != "Android" ]]; then
  depends+=(
    'cryptsetup-nested-cryptkey'
    'squashfs-tools'
  )
fi
if [[ "${_os}" != "GNU/Linux" ]] && \
   [[ "${_os}" == "Android" ]]; then
  depends+=(
    'cryptsetup'
    'squashfs-tools-ng'
    # "tsu"
    "sudo"
  )
fi
makedepends+=(
  "make"
)
if [[ "${_git}" == true ]]; then
  makedepends+=(
    "git"
  )
fi
if [[ "${_git}" == false ]]; then
  makedepends+=(
    "curl"
    "jq"
  )
fi
checkdepends=(
  "shellcheck"
)
_archiso_profiles_optdepends=(
  'archiso-profiles:'
    'Extra profiles for archiso.'
)
_edk2_ovmfs_optdepends=(
  'edk2-ovmf:'
    'For emulating UEFI with run_archiso.'
)
_openssl_optdepends=(
  'openssl:'
    'For codesigning support when building netboot artifacts.'
)
_qemu_optdepends=(
  'qemu:'
    "For 'run_archiso'."
)
optdepends=(
  "${_archiso_profiles_optdepends[*]}"
  "${_edk2_ovmf_optdepends[*]}"
  "${_openssl_optdepends[*]}"
  "${_qemu_optdepends[*]}"
)
if [[ "${_os}" == 'Android' ]]; then
  optdepends+=(
  )
fi
provides=(
  "${_pkg}=${pkgver}"
)
conflicts=(
  "${_pkg}"
)
groups=(
 "${_proj}"
 "${_proj}-git"
)
_url="${url}"
if [[ "${_local}" == true ]]; then
  _url="${_local}"
fi
source=()
_branch="raid"
_tarname="${_pkg}-${_branch}"
_tarfile="${_tarname}.tar.gz"
if [[ "${_git}" == "true" ]]; then
  _tarfile="${_tarname}"
  _uri="${_url}#branch=${_branch}"
elif [[ "${_git}" == "false" ]]; then
  _uri="${_url}/archive/refs/heads/${_branch}.tar.gz"
  # if [[ "${_git_service}" == "gitlab" ]]; then
  #   _uri="${_url}/archive/refs/heads/${_branch}.tar.gz"
  # if [[ "${_git_service}" == "gitlab" ]]; then
  #   _uri="loscopriremosolovivendo"
  # fi
fi
source=(
  "${_tarname}::git+${_uri}"
)

sha256sums=(
  "SKIP"
)

_nth() {
  local \
    _str="${1}" \
    _n="${2}"
  echo \
    "${_str}" |
    awk \
      -F \
        '+' \
      '{print $'"${_n}"'}'
}

_jq_pkgver() {
  local \
    _version \
    _rev \
    _commit
  _version="$(
    curl \
      --silent \
      "${_gh_api}/tags" |
      jq \
        '.[0].name')"
  _version_commit="$(
    curl \
      --silent \
      "${_gh_api}/tags" |
      jq \
        '.[0].commit.sha')"
  _rev="$( \
    curl \
      --silent \
      "${_gh_api}/commits" |
      jq \
        'map(.sha == '${_version_commit}' ) | index(true)')"
  _commit="$( \
    curl \
      --silent \
      "${_gh_api}/commits" |
      jq \
        '.[0].sha')"
  printf \
    "%s.r%s.g%s" \
    "${_version}" \
    "${_rev}" \
    "${_commit}"
}

_parse_ver() {
  local \
    _pkgver="${1}" \
    _out="" \
    _ver \
    _rev \
    _commit
  _ver="$(
    _nth \
      "${_pkgver}" \
      "1")"
  _rev="$(
    _nth \
      "${_pkgver}" \
      "2")"
  _commit="$(
    _nth \
      "${_pkgver}" \
      "3")"
  _out=${_ver}
  if [[ "${_rev}" != "" ]]; then
    _out+=".r${_rev}"
  fi
  if [[ "${_commit}" != "" ]]; then
    _out+=".${_commit}"
  fi
  echo \
    "${_out}"
}

_git_pkgver() {
  local \
    _pkgver
  _pkgver="$(
    git \
      describe \
      --tags \
      --long |
      sed \
        's/-/+/g')"
  _parse_ver \
    "${_pkgver}"
}

pkgver() {
  cd \
    "${_pkgname}-${_branch}"
  if [[ "${_git}" == true ]]; then
    _git_pkgver
  elif [[ "${_git}" == false ]]; then
    _jq_pkgver
  fi
}

package() {
  local \
    _make_opts=()
  _make_opts+=(
    DESTDIR="${pkgdir}"
    PREFIX="/usr"
  )
  cd \
    "${_tarname}"
  make \
    "${_make_opts[@]}" \
    install
}

# vim: ft=sh syn=sh et
