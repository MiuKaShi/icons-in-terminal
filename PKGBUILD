# Maintainer: A modify icons-terminal

pkgname=icons-terminal
pkgver=r94.3bab2c8
pkgrel=1
pkgdesc="Icon fonts in terminal without a need for replacing or patching existing"
url="https://github.com/sebastiencs/icons-in-terminal"
arch=('any')
license=('custom:MIT')
provides=('icons-in-terminal')
conflicts=('icons-in-terminal')
depends=('bash')
makedepends=('git' 'xmlstarlet')
source=(
    "$pkgname::git+https://github.com/MiuKaShi/icons-in-terminal.git"
    "$pkgname-PR50.patch::https://github.com/sebastiencs/icons-in-terminal/pull/50.patch")
sha1sums=('SKIP'
          '9d7df34c3c895df45d372bbd32bf5a6ddebc600c')

pkgver() {
    cd "$srcdir/$pkgname"
    printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
    cd "$srcdir/$pkgname"
    
    patch -p1 -i "../$pkgname-PR50.patch"

    sed -i 's filename="./build/mapping.txt" filename="/usr/share/icons-terminal/mapping.txt" ' print_icons.sh
    ./scripts/generate_fontconfig.sh > "30-$pkgname.conf"
}

package() {
    cd "$srcdir/$pkgname"

    if [ ! -f "/usr/share/fontconfig/conf.avail/30-$pkgname.conf" ]; then
        install -dm755 "$pkgdir/etc/fonts/conf.d"
        install -Dm644 "30-$pkgname.conf"       "$pkgdir/usr/share/fontconfig/conf.avail/30-$pkgname.conf"
        ln -rs "$pkgdir"/usr/share/fontconfig/conf.avail/* "$pkgdir/etc/fonts/conf.d/"
    fi

    install -Dm755 "print_icons.sh"         "$pkgdir/usr/bin/$pkgname"
    install -Dm644 "README.md"              "$pkgdir/usr/share/$pkgname/README.md"
    install -Dm644 "LICENSE"                "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
    install -Dm644 "build/icons-in-terminal.ttf"     "$pkgdir/usr/share/fonts/TTF/$pkgname.ttf"

    find build/ -type f ! -name "*.ttf" -exec install -m644 {} "$pkgdir/usr/share/$pkgname/" \;

    # install='icons-in-terminal.install'
}
