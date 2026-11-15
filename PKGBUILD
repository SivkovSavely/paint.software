# Builds the local checkout; the upstream-tracking recipe is in packaging/aur.
pkgname=paint.software-git
pkgver=r112.635a36b
pkgrel=1
pkgdesc='Open-source Paint.NET clone for Linux with layers, effects and plugins'
arch=('x86_64')
url='https://github.com/Univers4craft/paint.software'
license=('MIT')
# WebP and TIFF support comes from dynamically loaded qt6-imageformats plugins.
depends=('glibc' 'libgcc' 'libstdc++' 'qt6-base' 'qt6-imageformats' 'zlib')
optdepends=('qt6-translations: translated standard dialog buttons')
makedepends=('cmake' 'ninja' 'git')
provides=('paint.software')
conflicts=('paint.software')

# Keep makepkg --cleanbuild away from the project's own src/ directory.
BUILDDIR="$startdir/build/makepkg"

pkgver() {
  printf 'r%s.%s\n' "$(git -C "$startdir" rev-list --count HEAD)" \
    "$(git -C "$startdir" rev-parse --short HEAD)"
}

build() {
  cmake -S "$startdir" -B "$srcdir/build" -G Ninja \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DPAINTSW_VERSION="$pkgver"
  cmake --build "$srcdir/build"
}

check() {
  cmake --build "$srcdir/build" --target \
    paintdotnet_tests paintdotnet_brush_tests \
    paintdotnet_paste_tests paintdotnet_preview_tests
  QT_QPA_PLATFORM=offscreen ctest --test-dir "$srcdir/build" --output-on-failure
}

package() {
  DESTDIR="$pkgdir" cmake --install "$srcdir/build"
  install -Dm644 "$startdir/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
