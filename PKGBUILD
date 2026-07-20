pkgname=greetd-dms-greeter-git
pkgver=0
pkgrel=1
pkgdesc="DankMaterialShell greeter for greetd (Git)"
arch=('x86_64')
url="https://github.com/AvengeMedia/dank-greeter"
license=('MIT')

depends=(
    'greetd'
    'quickshell'
)

makedepends=(
    'git'
    'go'
    'make'
)

optdepends=(
    'niri: Niri compositor support'
    'hyprland: Hyprland compositor support'
    'sway: Sway compositor support'
)

provides=('greetd-dms-greeter')
conflicts=(
    'greetd-dms-greeter'
    'greetd-dms-greeter-bin'
)

source=(
    "$pkgname::git+https://github.com/AvengeMedia/dank-greeter.git"
)

sha256sums=('SKIP')

pkgver() {
    cd "$srcdir/$pkgname"

    if git describe --tags --long >/dev/null 2>&1; then
        git describe --tags --long | sed 's/^v//; s/-/./g'
    else
        printf "r%s.%s" \
            "$(git rev-list --count HEAD)" \
            "$(git rev-parse --short HEAD)"
    fi
}

prepare() {
    cd "$srcdir/$pkgname"

    git submodule update --init --recursive
}

build() {
    cd "$srcdir/$pkgname"

    export GOAMD64=v3
    export GOFLAGS="${GOFLAGS} -trimpath"

    make build
}

package() {
    cd "$srcdir/$pkgname"

    # Binary
    install -Dm755 \
        core/bin/dms-greeter \
        "$pkgdir/usr/bin/dms-greeter"

    # Quickshell assets
    install -dm755 \
        "$pkgdir/usr/share/quickshell/dms-greeter"

    cp -a quickshell/. \
        "$pkgdir/usr/share/quickshell/dms-greeter/"

    # Documentation
    install -Dm644 \
        README.md \
        "$pkgdir/usr/share/doc/$pkgname/README.md"

    # Runtime cache
    install -dm750 \
        "$pkgdir/var/cache/dms-greeter"
}