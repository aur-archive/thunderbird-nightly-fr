# Maintainer: Bruno Pagani (a.k.a. ArchangeGabriel) <bruno.n.pagani at gmail dot com>

_name=thunderbird
_channel=nightly
_lang=fr
_full_name=${_name}-${_channel}
#pkgname=${_full_name}-${_lang}
pkgname=thunderbird-nightly-fr
pkgdesc="Standalone Mail/News reader from Mozilla – Nightly build (fr)"
url="https://www.mozilla.org/thunderbird"
_version=40.0a1
#pkgver=${_version}.yyyymmdd
pkgver=40.0a1.20150401
pkgrel=1
arch=('i686' 'x86_64')
license=('MPL' 'GPL' 'LGPL')
depends=('alsa-lib' 'dbus-glib' 'desktop-file-utils' 'gtk2' 'libxt' 'nss' 'mime-types')
optdepends=('hunspell: spell checking'
            'hyphen: hyphenation')
_base_src="${_name}-${_version}.${_lang}.linux-${CARCH}"
_base_url="https://ftp.mozilla.org/pub/mozilla.org/${_name}/nightly/latest-comm-central-l10n"
_tarball="${_base_src}.tar.bz2"
source=("${_base_url}/${_tarball}" 'thunderbird-nightly.desktop' 'vendor.js')
_checksum="$(curl -s "${_base_url}/${_base_src}.checksums" | grep ${_tarball} | grep sha512 | cut -d " " -f1)"
sha512sums=("${_checksum}" '004ed624306ed96c7873e42f2ebeeccbaf3863efc97c372cdfdc3b396bae20b7370974a2ec7ce6779e350bd5957388a57f462e680bba9b91ae54abd942e248bd' 'bae5a952d9b92e7a0ccc82f2caac3578e0368ea6676f0a4bc69d3ce276ef4f70802888f882dda53f9eb8e52911fb31e09ef497188bcd630762e1c0f5293cc010')
install=thunderbird-nightly.install

pkgver() {
  SRC_VER="${_name}-${_version}.en-US.linux-${CARCH}.txt"
  curl -OR "https://ftp.mozilla.org/pub/mozilla.org/${_name}/nightly/latest-comm-central/${SRC_VER}"
  echo "${_version}.$(head -n1 ${SRC_VER} | cut -c -8)"
}

# Uncomment check() to enable GnuPG signature verification. You’ll need Mozilla’s GnuPG release key.
# Their current fingerprint is 2B90 598A 745E 992F 315E  22C5 8AB1 3296 3A06 537A shortid 0x15A0A4BC
#check() {
#  CHECKSUM="${_base_src}.checksums"
#  CHECKSIG="${CHECKSUM}.asc"
#  curl -OR "${_base_url}/${CHECKSUM}"
#  curl -OR "${_base_url}/${CHECKSIG}"
#  gpg --verify ${CHECKSIG} ${CHECKSUM}
#}

package() {
  install -d "${pkgdir}"/{usr/bin,opt}

  OPT_PATH="/opt/${_name}-${_version}"

  cp -r thunderbird "${pkgdir}/${OPT_PATH}"
  ln -s "${OPT_PATH}/thunderbird" "${pkgdir}/usr/bin/${_full_name}"

  # Install icons
  for i in 16 22 24 32 48 256; do
      install -Dm644 "${srcdir}/thunderbird/chrome/icons/default/default${i}.png" "${pkgdir}/usr/share/icons/hicolor/${i}x${i}/apps/${_full_name}.png"
  done

  install -Dm644 ${_full_name}.desktop "$pkgdir/usr/share/applications/${_full_name}.desktop"
  # Deactivate auto-updates
  install -Dm644 "${srcdir}/vendor.js" "${pkgdir}/${OPT_PATH}/defaults/pref/vendor.js"

  # Use system-provided dictionaries
  rm -rf "${pkgdir}/${OPT_PATH}"/{dictionaries,hyphenation}
  ln -sf /usr/share/hunspell "${pkgdir}/${OPT_PATH}/dictionaries"
  ln -sf /usr/share/hyphen "${pkgdir}/${OPT_PATH}/hyphenation"
}
