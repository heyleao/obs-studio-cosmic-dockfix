# Maintainer: Bruno <bruno@local>

_suffix=cosmic-dockfix
pkgname="obs-studio-${_suffix}"
_pkgver=32.1.2
pkgver="${_pkgver//-/_}"
pkgrel=1
pkgdesc="OBS Studio with CEF and COSMIC/XWayland dock workaround"
arch=("x86_64" "aarch64")
url="https://github.com/obsproject/obs-studio"
license=('GPL-2.0-or-later')
_qtver=6.11
_libajantv2ver=17.5.0
_libdatachannelver=0.24.3
_mbedtlsver=3.6.1
_pythonver=3.14
depends=(
  "alsa-lib"
  "curl"
  "ffmpeg>=8"
  "fontconfig"
  "freetype2"
  "gcc-libs"
  "glib2"
  "glibc"
  "jack"
  "jansson"
  "libajantv2>=$_libajantv2ver"
  "libdatachannel>=$_libdatachannelver"
  "libfdk-aac"
  "libgl"
  "libpipewire"
  "libpulse"
  "librist"
  "libva"
  "libvpl"
  "libx11"
  "libxcb"
  "libxcomposite"
  "libxkbcommon"
  "luajit"
  "mbedtls>=$_mbedtlsver"
  "pciutils"
  "python>=$_pythonver"
  "qrcodegencpp-cmake"
  "qt6-base>=$_qtver"
  "qt6-svg>=$_qtver"
  "rnnoise"
  "simde"
  "sndio"
  "speexdsp"
  "srt"
  "systemd-libs"
  "util-linux-libs"
  "v4l-utils"
  "libvlc-luajit"
  "wayland"
  "x264"
  "zlib"
  "at-spi2-core" "cairo" "dbus" "expat" "libcups" "libdrm"
  "libxdamage" "libxext" "libxfixes" "libxrandr" "mesa" "nspr"
  "nss" "pango"
)
makedepends=(
  "asio"
  "cmake"
  "extra-cmake-modules"
  "ffnvcodec-headers"
  "git"
  "uthash"
  "nlohmann-json"
  "swig"
  "websocketpp"
)
optdepends=(
  "intel-media-sdk: QSV encoder support(<= Rocket Lake & >= Broadwell)"
  "vpl-gpu-rt: QSV encoder support (>= Alder Lake)"
  "intel-media-driver: VAAPI encoder support (>= Broadwell)"
  "libva-intel-driver: VAAPI encoder support (<= Haswell)"
  "libva-mesa-driver: VAAPI encoder support"
  "v4l2loopback-dkms: V4L2 virtual camera output"
)
provides=(
  "obs-studio=$pkgver" "obs-vst" "obs-websocket" "obs-browser"
  "obs-studio-plugin-browser"
)
conflicts=(
  "obs-studio" "obs-vst" "obs-websocket" "obs-browser"
  "obs-linuxbrowser"
  "libva-vdpau-driver"
  "obs-studio-plugin-browser"
  "obs-studio-browser"
)
options=('debug')
source=(
  "obs-studio::git+https://github.com/obsproject/obs-studio.git#tag=$_pkgver"
  "obs-browser::git+https://github.com/obsproject/obs-browser.git"
  "obs-websocket::git+https://github.com/obsproject/obs-websocket.git"
  "obs-browser-nvidia-x11-cef.patch"
  "obs-cosmic-no-floating-docks.patch"
  "obs-studio-cosmic-dockfix"
  "obs-studio-cosmic-dockfix.desktop"
)
source_x86_64=("https://cdn-fastly.obsproject.com/downloads/cef_binary_6533_linux_x86_64_v6.tar.xz")
source_aarch64=("https://cdn-fastly.obsproject.com/downloads/cef_binary_6533_linux_aarch64_v6.tar.xz")
sha256sums=(
  "SKIP"
  "SKIP"
  "SKIP"
  "SKIP"
  "SKIP"
  "SKIP"
  "SKIP"
)
sha256sums_x86_64=("7963335519a19ccdc5233f7334c5ab023026e2f3e9a0cc417007c09d86608146")
sha256sums_aarch64=("642514469eaa29a5c887891084d2e73f7dc2d7405f7dfa7726b2dbc24b309999")

if [[ ${CARCH/%_v?/} == 'x86_64' ]]; then
  optdepends+=("decklink: Blackmagic Design DeckLink support")
fi

prepare() {
  cd "$srcdir/obs-studio"
  git config submodule.plugins/obs-browser.url "$srcdir/obs-browser"
  git config submodule.plugins/obs-websocket.url "$srcdir/obs-websocket"
  git -c protocol.file.allow=always submodule update

  patch -Np1 -i "$srcdir/obs-browser-nvidia-x11-cef.patch"
  patch -Np1 -i "$srcdir/obs-cosmic-no-floating-docks.patch"

  sed -i "s|obs_get_version_string()|\"$_pkgver-$_suffix-$pkgrel\"|" frontend/OBSApp.cpp
  sed -i "s|#ifndef NDEBUG|#if 0|" frontend/utility/CrashHandler.cpp
}

build() {
  local cmake_options=(
    -B build
    -S obs-studio
    -DCMAKE_BUILD_TYPE=None
    -DCMAKE_INSTALL_PREFIX=/usr
    -DCMAKE_INSTALL_LIBDIR=lib
    -DENABLE_LIBFDK=ON
    -DENABLE_JACK=ON
    -DENABLE_SNDIO=ON
    -DENABLE_BROWSER=ON
    -DCEF_ROOT_DIR="$srcdir/cef_binary_6533_linux_${CARCH/%_v?/}"
    -DOBS_VERSION_OVERRIDE="$_pkgver"
    -DOBS_COMPILE_DEPRECATION_AS_WARNING=ON
    -Wno-dev
  )

  # Optional service integration. Public builds cannot ship OBS's private OAuth
  # credentials. Builders may provide compatible values at build time:
  #   TWITCH_CLIENTID=... TWITCH_HASH=... makepkg -si
  [[ -n "${TWITCH_CLIENTID:-}" ]] && cmake_options+=("-DTWITCH_CLIENTID=$TWITCH_CLIENTID")
  [[ -n "${TWITCH_HASH:-}" ]] && cmake_options+=("-DTWITCH_HASH=$TWITCH_HASH")
  [[ -n "${RESTREAM_CLIENTID:-}" ]] && cmake_options+=("-DRESTREAM_CLIENTID=$RESTREAM_CLIENTID")
  [[ -n "${RESTREAM_HASH:-}" ]] && cmake_options+=("-DRESTREAM_HASH=$RESTREAM_HASH")
  [[ -n "${YOUTUBE_CLIENTID:-}" ]] && cmake_options+=("-DYOUTUBE_CLIENTID=$YOUTUBE_CLIENTID")
  [[ -n "${YOUTUBE_CLIENTID_HASH:-}" ]] && cmake_options+=("-DYOUTUBE_CLIENTID_HASH=$YOUTUBE_CLIENTID_HASH")
  [[ -n "${YOUTUBE_SECRET:-}" ]] && cmake_options+=("-DYOUTUBE_SECRET=$YOUTUBE_SECRET")
  [[ -n "${YOUTUBE_SECRET_HASH:-}" ]] && cmake_options+=("-DYOUTUBE_SECRET_HASH=$YOUTUBE_SECRET_HASH")

  cmake "${cmake_options[@]}"
  cmake --build build
}

package() {
  DESTDIR="$pkgdir" cmake --install build
  install -Dm755 "$srcdir/obs-studio-cosmic-dockfix" "$pkgdir/usr/bin/obs-studio-cosmic-dockfix"
  install -Dm644 "$srcdir/obs-studio-cosmic-dockfix.desktop" \
    "$pkgdir/usr/share/applications/obs-studio-cosmic-dockfix.desktop"
}
