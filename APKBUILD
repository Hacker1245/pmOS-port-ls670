# Reference: <https://postmarketos.org/devicepkg>
pkgname="device-lg-ls670"
pkgdesc="LG Optimus S"
pkgver=0.1
pkgrel=0
url="https://postmarketos.org"
license="MIT"
arch="noarch"
options="!check"
depends="postmarketos-base linux-lg-ls670 mkbootimg mesa-dri-swrast"
makedepends="devicepkg-dev"
source="deviceinfo"

build() {
	devicepkg_build $startdir $pkgname
}

package() {
	devicepkg_package $startdir $pkgname
}

sha512sums="(run 'pmbootstrap checksum device-lg-ls670' to fill)"

