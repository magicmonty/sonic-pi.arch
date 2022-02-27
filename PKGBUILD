# Mainainer: Martin Gondermann <magicmonty@pagansoft.de>

_name=sonic-pi
pkgname=sonic-pi-magicmonty-git
pkgver=v3.3.1.r774.g1acae4317
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
  ./linux-prebuild.sh -n
  cat ./linux-config.sh | sed -e 's/config=""/config=""\nno_imgui=true/g' >> ./linux-config-no-imgui.sh
  chmod +x ./linux-config-no-imgui.sh
  ./linux-config-no-imgui.sh
  cd build
  cmake --build . --config Release
}

package() {
  cd "${_name}"

  # GUI executable
  install -vDm 755 bin/${_name} "${pkgdir}/opt/${_name}/bin/${_name}"
  install -vDm 755 app/build/gui/qt/${_name} "${pkgdir}/opt/${_name}/app/build/gui/qt/${_name}"
  install -vDm 644 app/build/api/*.a -t "${pkgdir}/opt/${_name}/app/build/api/libsonic-pi-api.a"

  # book
  install -vDm 644 app/gui/qt/book/*.html -t "${pkgdir}/opt/${_name}/app/gui/qt/book"
  install -vDm 644 app/gui/qt/lang/*.qm -t "${pkgdir}/opt/${_name}/app/gui/qt/lang"
  install -vDm 644 app/gui/qt/help/*.html -t "${pkgdir}/opt/${_name}/app/gui/qt/help"
  install -vDm 644 app/gui/qt/html/*.html -t "${pkgdir}/opt/${_name}/app/gui/qt/html"
  install -vDm 644 app/gui/qt/fonts/*.{ttf,md} -t "${pkgdir}/opt/${_name}/app/gui/qt/fonts"
  # images
  install -vDm 644 app/gui/qt/images/*.png -t "${pkgdir}/opt/${_name}/app/gui/qt/images"
  install -vDm 644 app/gui/qt/images/coreteam/*.png -t "${pkgdir}/opt/${_name}/app/gui/qt/images/coreteam"
  install -vDm 644 app/gui/qt/images/toolbar/pro/*.png -t "${pkgdir}/opt/${_name}/app/gui/qt/images/toolbar/pro"
  install -vDm 644 app/gui/qt/images/tutorial/*.png -t "${pkgdir}/opt/${_name}/app/gui/qt/images/tutotial"
  # theme
  install -vDm 644 app/gui/qt/theme/*.qss -t "${pkgdir}/opt/${_name}/app/gui/qt/theme"
  install -vDm 644 app/gui/qt/theme/dark/doc-styles.css -t "${pkgdir}/opt/${_name}/app/gui/qt/theme/dark"
  install -vDm 644 app/gui/qt/theme/light/doc-styles.css -t "${pkgdir}/opt/${_name}/app/gui/qt/theme/light"
  install -vDm 644 app/gui/qt/theme/high_contrast/doc-styles.css -t "${pkgdir}/opt/${_name}/app/gui/qt/theme/light"
  # samples
  install -vDm 644 etc/samples/*.{flac,md} -t "${pkgdir}/opt/${_name}/etc/samples"
  # snippets
  install -vDm 644 etc/snippets/fx/*.sps -t "${pkgdir}/opt/${_name}/etc/snippets/fx"
  install -vDm 644 etc/snippets/live_loop/*.sps -t "${pkgdir}/opt/${_name}/etc/snippets/live_loop"
  install -vDm 644 etc/snippets/syntax/*.sps -t "${pkgdir}/opt/${_name}/etc/snippets/syntax"
  # synth defs
  install -vDm 644 etc/synthdefs/compiled/*.scsyndef -t "${pkgdir}/opt/${_name}/etc/synthdefs/compiled"
  # user examples
  install -vDm 644 app/config/user-examples/* -t "${pkgdir}/opt/${_name}/app/config/user-examples"
  install -vDm 644 app/config/user-examples/* -t "${pkgdir}/etc/skel/.sonic-pi/config"
  install -vDm 644 etc/buffers/*.wav -t "${pkgdir}/opt/${_name}/etc/buffers"
  # examples
  install -vDm 644 etc/examples/algomancer/*.rb -t "${pkgdir}/opt/${_name}/etc/examples/algomancer"
  install -vDm 644 etc/examples/apprentice/*.rb -t "${pkgdir}/opt/${_name}/etc/examples/apprentice"
  install -vDm 644 etc/examples/illusionist/*.rb -t "${pkgdir}/opt/${_name}/etc/examples/illusionist"
  install -vDm 644 etc/examples/incubation/*.rb -t "${pkgdir}/opt/${_name}/etc/examples/incubation"
  install -vDm 644 etc/examples/magician/*.rb -t "${pkgdir}/opt/${_name}/etc/examples/magician"
  install -vDm 644 etc/examples/sorcerer/*.rb -t "${pkgdir}/opt/${_name}/etc/examples/sorcerer"
  install -vDm 644 etc/examples/wizard/*.rb -t "${pkgdir}/opt/${_name}/etc/examples/wizard"
  # erlang
  install -vDm 755 app/server/beam/tau/boot-lin.sh "${pkgdir}/opt/${_name}/app/server/beam/tau/boot-lin.sh"
  cp -arv app/server/beam/tau/assets "${pkgdir}/opt/${_name}/app/server/beam/tau/"
  install -vD app/server/beam/tau/ebin/*.app -t "${pkgdir}/opt/${_name}/app/server/beam/tau/ebin"
  cp -arv app/server/beam/tau/lib ${pkgdir}/opt/${_name}/app/server/beam/tau/
  cp -arv app/server/beam/tau/priv ${pkgdir}/opt/${_name}/app/server/beam/tau/
  install -vD app/server/beam/tau/rel/*.eex -t "${pkgdir}/opt/${_name}/app/server/beam/tau/rel"
  install -vD app/server/beam/tau/config/*.exs -t "${pkgdir}/opt/${_name}/app/server/beam/tau/config"
  cp -arv app/server/beam/tau/_build "${pkgdir}/opt/${_name}/app/server/beam/tau/"
  # native 
  install -vDm 755 app/server/native/aubio_onset "${pkgdir}/opt/${_name}/app/server/native/aubio_onset"
  # ruby
  install -vdm 755 ${pkgdir}/opt/${_name}/app/server/ruby
  cp -av app/server/ruby/* "${pkgdir}/opt/${_name}/app/server/ruby"
  rm -vf ${pkgdir}/opt/${_name}/app/server/ruby/vendor/*/ext/*.{o,c,h}
  rm -vf ${pkgdir}/opt/${_name}/app/server/ruby/vendor/*/ext/*/*.{o,c,h}
  rm -vf ${pkgdir}/opt/${_name}/app/server/ruby/vendor/*/ext/Makefile
  rm -vf ${pkgdir}/opt/${_name}/app/server/ruby/vendor/*/ext/*/Makefile
  rm -vrf ${pkgdir}/opt/${_name}/app/server/ruby/vendor/rugged-*/vendor/libgit2/{azure-pipelines,cmake,deps,docs,examples,fuzzers,include,script,src/tests}
  rm -vrf ${pkgdir}/opt/${_name}/app/server/ruby/vendor/rugged-*/vendor/libgit2/build/{CMakeFiles,deps,src}
  rm -vrf ${pkgdir}/opt/${_name}/app/server/ruby/vendor/*/test
  rm -vf ${pkgdir}/opt/${_name}/app/server/ruby/Rakefile
  rm -vrf ${pkgdir}/opt/${_name}/app/server/ruby/test
  rm -vf ${pkgdir}/opt/${_name}/app/server/ruby/vendor/*/Rakefile
  # xdg
  install -vDm 644 ${_name}.desktop -t "${pkgdir}/usr/share/applications/"
  install -vDm 644 app/gui/qt/images/icon-smaller.png "${pkgdir}/usr/share/icons/${_name}.png"
  # license
  install -vDm 644 LICENSE.md -t "${pkgdir}/usr/share/licenses/${_name}/LICENSE"

  install -vdm 755 ${pkgdir}/opt/${_name}/app/server/native/sox
  ln -s /usr/bin/sox ${pkgdir}/opt/${_name}/app/server/native/sox/sox
  
  install -vDm 755 ../${_name}.sh "${pkgdir}/usr/bin/${_name}"
}
