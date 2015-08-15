# Maintainer: Mark Pustjens <pustjens@dds.nl>
# Contributor: Patrick Chilton <chpatrick _at_ gmail _dot_ com>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Daenyth <Daenyth+Arch [AT] gmail [DOT] com>
# Contributor: djnm <nmihalich [at} gmail dott com>
pkgname=dwarffortress-mayday
pkgver=0.40.13
_dfver=40_13
_dfgver=40_05
pkgrel=1
pkgdesc="A single-player fantasy game. You control a dwarven outpost or an adventurer in a randomly generated persistent world. Mayday tileset."
arch=(i686 x86_64)
# WIP Thread: http://www.bay12forums.com/smf/index.php?topic=66142.0
url="http://mayday.w.staszic.waw.pl/df.php"
install="$pkgname.install"
license=('custom:dwarffortress')
depends=(gtk2 glu sdl_image libsndfile openal sdl_ttf glew gcc-libs)
makedepends=(git cmake unzip git)
options=('!strip')
if [[ $CARCH == 'x86_64' ]]; then
  makedepends+=(gcc-multilib)
  depends=(gcc-libs-multilib lib32-gtk2 lib32-glu lib32-sdl_image lib32-libsndfile lib32-openal
           lib32-libxdamage lib32-ncurses lib32-sdl_ttf lib32-glew)
  optdepends=('lib32-nvidia-utils: If you have nvidia graphics'
              'lib32-catalyst-utils: If you have ATI graphics'
              'lib32-alsa-lib: for alsa sound'
              'lib32-libpulse: for pulse sound')
fi
backup=('opt/dwarffortress-mayday/data/init/colors.txt'
        'opt/dwarffortress-mayday/data/init/init.txt'
        'opt/dwarffortress-mayday/data/init/d_init.txt'
        'opt/dwarffortress-mayday/data/init/interface.txt')
noextract=(dfg_${_dfgver}_win.zip df_${_dfver}_linux.tar.bz2)
# I made a fucking github repo with the sole purpose of unfucking df a bit
# We try to compile whatever little bit of df is open source
source=(http://www.bay12games.com/dwarves/df_${_dfver}_linux.tar.bz2
        http://mayday.w.staszic.waw.pl/~mayday/upload/DFG/dfg_${_dfgver}_win.zip
        git://github.com/svenstaro/dwarf_fortress_unfuck.git
        dwarffortress
        dwarffortress.desktop
        dwarffortress.png)
sha256sums=('a75accc5b0dad5311d8eefdc3e40c55710cef83619f343f5638a025fd220d541'
            '7c98c31e5b7486b9b66e4a275df6963623fec7fc75a1803d020ec3101a0b9296'
            'SKIP'
            '4a7cc39dabe4fa3c28fdb9bdff83f7cdf55de0beff700fd6ebe605dda2f81688'
            'e79e3d945c6cc0da58f4ca30a210c7bf1bc3149fd10406d1262a6214eb40445a'
            '83183abc70b11944720b0d86f4efd07468f786b03fa52fe429ca8e371f708e0f')

build() {
  cd $srcdir/dwarf_fortress_unfuck

  cmake .
  make
}

package() {
  cd "${srcdir}"
  tar -f "df_${_dfver}_linux.tar.bz2" -x \
  --exclude "df_linux/raw" \
  --exclude "df_linux/data/art" \
  --exclude "df_linux/data/init/colors.txt" \
  --exclude "df_linux/data/init/init.txt"

  unzip -qod df_linux "dfg_${_dfgver}_win.zip" \
    "raw/*" \
    "data/art/*" \
    "data/init/colors.txt" \
    "data/init/init.txt"

  cd $srcdir/df_linux
  install -dm755 $pkgdir/opt/
  cp -r $srcdir/df_linux $pkgdir/opt/$pkgname
  rm -r $pkgdir/opt/$pkgname/df $pkgdir/opt/$pkgname/libs/* $pkgdir/opt/$pkgname/g_src

  find $pkgdir/opt/$pkgname -type d -exec chmod 755 {} +
  find $pkgdir/opt/$pkgname -type f -exec chmod 644 {} +

  install -Dm755 $srcdir/df_linux/libs/Dwarf_Fortress $pkgdir/opt/$pkgname/libs/Dwarf_Fortress
  install -Dm755 $srcdir/dwarf_fortress_unfuck/libgraphics.so $pkgdir/opt/$pkgname/libs/libgraphics.so
  install -Dm755 $srcdir/dwarffortress $pkgdir/usr/bin/$pkgname

  # No idea why we need this. Really. This isn't being loaded dynamically, it's not linked and
  # in general there is no indication this is being used. However, it doesn't work without this symlink.
  [[ $CARCH == "x86_64" ]] && ln -s /usr/lib32/libpng.so $pkgdir/opt/$pkgname/libs/libpng.so.3
  [[ $CARCH == "i686" ]] && ln -s /usr/lib/libpng.so $pkgdir/opt/$pkgname/libs/libpng.so.3

  # Desktop launcher with icon
  install -Dm644 $srcdir/dwarffortress.desktop $pkgdir/usr/share/applications/"$pkgname".desktop
  install -Dm644 $srcdir/dwarffortress.png     $pkgdir/usr/share/pixmaps/"$pkgname".png

  install -Dm644 $srcdir/df_linux/readme.txt $pkgdir/usr/share/licenses/$pkgname/readme.txt

  # Set pkgname in runscript / desktop file
  sed -i "s/^pkgname=.*/pkgname=$pkgname/" $pkgdir/usr/bin/$pkgname
  sed -i "s/^exec=.*/exec=$pkgname/" $pkgdir/usr/share/applications/$pkgname.desktop
}

# vim:set ts=2 sw=2 et:
sha256sums=('c17c021150e8e84c6bac949007ebed6383d9766d279a1b088808bac0e4ebfaab'
            '7c98c31e5b7486b9b66e4a275df6963623fec7fc75a1803d020ec3101a0b9296'
            'SKIP'
            '4a7cc39dabe4fa3c28fdb9bdff83f7cdf55de0beff700fd6ebe605dda2f81688'
            'e79e3d945c6cc0da58f4ca30a210c7bf1bc3149fd10406d1262a6214eb40445a'
            '83183abc70b11944720b0d86f4efd07468f786b03fa52fe429ca8e371f708e0f')
