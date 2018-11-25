# Reference: <https://postmarketos.org/vendorkernel>
# Kernel config based on: arch/arm/configs/(CHANGEME!)

pkgname="linux-lg-ls670"
pkgver=2.6.35
pkgrel=0
pkgdesc="LG Optimus S kernel fork"
arch="armhf"
_carch="arm"
_flavor="lg-ls670"
url="https://kernel.org"
license="GPL-2.0-only"
options="!strip !check !tracedeps"
makedepends="perl sed installkernel bash gmp-dev bc linux-headers elfutils-dev devicepkg-dev"

# Compiler: latest GCC from Alpine
HOSTCC="${CC:-gcc}"
HOSTCC="${HOSTCC#${CROSS_COMPILE}}"

# Source
_repository="GingerKernel-thunderc"
_commit="8a848fa4880280cdd0eb8c47c9fa3483312df718"
_config="config-${_flavor}.${arch}"
source="
	$pkgname-$_commit.tar.gz::https://github.com/Earthnfire78/${_repository}/archive/${_commit}.tar.gz
	$_config
	gcc7-give-up-on-ilog2-const-optimizations.patch
	gcc8-fix.patch
	00_fix_return_address.patch
	lg.patch
	
"
builddir="$srcdir/${_repository}-${_commit}"

prepare() {
	default_prepare
	downstreamkernel_prepare "$srcdir" "$builddir" "$_config" "$_carch" "$HOSTCC"
}

build() {
	unset LDFLAGS
	make ARCH="$_carch" CC="${CC:-gcc}" \
		KBUILD_BUILD_VERSION="$((pkgrel + 1 ))-postmarketOS"
}

package() {
	# kernel.release
	install -D "$builddir/include/config/kernel.release" \
		"$pkgdir/usr/share/kernel/$_flavor/kernel.release"

	# zImage (find the right one)
	cd "$builddir/arch/$_carch/boot"
	_target="$pkgdir/boot/vmlinuz-$_flavor"
	for _zimg in zImage-dtb Image.gz-dtb *zImage Image; do
		[ -e "$_zimg" ] || continue
		msg "zImage found: $_zimg"
		install -Dm644 "$_zimg" "$_target"
		break
	done
	if ! [ -e "$_target" ]; then
		error "Could not find zImage in $PWD!"
		return 1
	fi
}

sha512sums="d6d9505eac35038ab1218f40630eb9608627a27edce5eb6e78f6b66b523020b700ebd550551df1f015fa72b995d7b2ae108bcf530ca6a3938d87ef952533b673  linux-lg-ls670-8a848fa4880280cdd0eb8c47c9fa3483312df718.tar.gz
0040f223d099f1ad2920d849776a0698942a16e55ad917e70faa1cdd34db31798990905ad3fcfe30a0d30ec3d3677b1ddf1707d57f567484c7bf5cde944641d5  config-lg-ls670.armhf
77eba606a71eafb36c32e9c5fe5e77f5e4746caac292440d9fb720763d766074a964db1c12bc76fe583c5d1a5c864219c59941f5e53adad182dbc70bf2bc14a7  gcc7-give-up-on-ilog2-const-optimizations.patch
3ea7b5506b0bab388a957aabe43922d25df287ace2bd38b4f4635abd2c33b57b19a8bea555acaec39af9d6a59d42bd6f197705c4a861eb8c2c4b0c1f206e0e54  gcc8-fix.patch
ea1d3b5a234fa565e3c1a792de48f4fc4e6023d281d303c8e319c7ef28edc5739ab0e4dea0139a41f0a5c7d03e27921ccaa214fd0ac5c72245a094ce60128864  00_fix_return_address.patch
318ba3829cd42171c57553f941c8685ada845ed9ed20f09d3d10683b9326601a44d5d062f304f1108d6c6c02966b3cbda7d83c8ad9d56e279c83f5679f5b123e  lg.patch"
