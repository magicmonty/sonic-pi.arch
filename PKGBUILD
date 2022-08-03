# Mainainer: Martin Gondermann <magicmonty@pagansoft.de>

_name=sonic-pi
pkgname=sonic-pi-magicmonty-git
pkgver=v4.0.3.r0.gd3179d02f
pkgrel=1
pkgdesc="The Live Coding Music Synth for Everyone"
arch=('i686' 'x86_64')
url="https://sonic-pi.net/"
license=('MIT')
groups=('pro-audio')
conflicts=('sonic-pi')
provides=('sonic-pi')
depends=('erlang-nox' 'sox' 'elixir' 'gcc-libs' 'glibc' 'libx11' 'libxft' 'libxext' 'ruby' 'ruby-rexml' 'supercollider' 'sc3-plugins' 'rtmidi')
makedepends=('ninja' 'cmake' 'python' 'qt5-tools' 'gendesk')
source=(
  'git+https://github.com/sonic-pi-net/sonic-pi.git'
  'sonic-pi.sh')
sha512sums=(
  'SKIP'
  'b4e6d43bac5a1dded472fe8ad6107887446dc693fb5537980a91b8285b984af14af43ffb62d185b63a98d79674e713ae2668062e2c9460afcfb1f81606dc5d05')

pkgver() {
  cd "${_name}"
  git describe --long --tags | sed -r 's/([^-]*-g)/r\1/;s/-/./g'
}

prepare() {
  cd "${_name}"
  gendesk -n -f \
    --pkgname "${_name}" \
    --pkgdesc "${pkgdesc}" \
    --name "${_name}" \
    --exec "${_name}" \
    --categories "AudioVideo;Audio"
}

build() {
  cd "${_name}/app"
  ./linux-build-all.sh -n
}

package() {
  cd "${_name}/app"

  echo "Creating Linux release..."
  ./linux-release.sh

  mkdir -p "${pkgdir}/opt/${_name}"
  cp -r build/linux_dist/* "${pkgdir}/opt/${_name}/"

  cd ..

  # xdg
  install -vDm 644 ${_name}.desktop -t "${pkgdir}/usr/share/applications/"
  install -vDm 644 app/gui/qt/images/icon-smaller.png "${pkgdir}/usr/share/icons/${_name}.png"
  # license
  install -vDm 644 LICENSE.md -t "${pkgdir}/usr/share/licenses/${_name}/LICENSE"

  install -vDm 755 ../${_name}.sh "${pkgdir}/usr/bin/${_name}"
}
