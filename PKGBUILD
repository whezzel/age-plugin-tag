# Maintainer: Whezzel
_pkgname=age
pkgname=age-plugin-tag
pkgver=1.3.2
pkgrel=1
pkgdesc='age plugins for tag recipients (age1tag1/age1tagpq1), e.g. for sops with age-plugin-tpm'
arch=('x86_64')
url="https://github.com/FiloSottile/age"
license=('BSD-3-Clause')
depends=('glibc' 'age')
makedepends=('go')
source=("$_pkgname-$pkgver.tar.gz::$url/archive/v$pkgver.tar.gz")
sha512sums=('670d24d07a6834c20f11dcbd390880a78652ba4cb1fb3c2b86d9d105e3d36c8af939bbca1f9278a4338671b7d9953deb9931232f978b0776796f139bcf12a6d7')

build() {
    cd "$_pkgname-$pkgver"
    export CGO_CPPFLAGS="$CPPFLAGS" CGO_CFLAGS="$CFLAGS" CGO_CXXFLAGS="$CXXFLAGS" CGO_LDFLAGS="$LDFLAGS"
    go build \
      -trimpath \
      -buildmode=pie \
      -mod=readonly \
      -modcacherw \
      -ldflags "-X main.Version=$pkgver -linkmode external -extldflags \"$LDFLAGS\"" \
      -o . ./extra/age-plugin-tag ./extra/age-plugin-tagpq
}

package() {
    cd "$_pkgname-$pkgver"
    install -Dm755 -t "$pkgdir/usr/bin/" age-plugin-tag age-plugin-tagpq
    install -Dm644 -t "$pkgdir/usr/share/licenses/$pkgname/" LICENSE
}
