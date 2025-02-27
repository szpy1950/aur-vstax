# Maintainer: Fabien Dubosson <fabien.dubosson@gmail.com>

# Contributors:
# Pierre-Yves Savioz <savioz.py@protonmail.com>
# Stéphane Donnet <donnet.stephane@gmail.com>

pkgname="vstax"
_year="2024"
pkgver="${_year}_1.0.0"
pkgrel="113"
pkgdesc="Software to fill the tax forms of the canton of Valais, Switzerland"
url="http://www.vs.ch/vstax"
license=('unknown')
arch=('x86_64')
depends=('java-runtime' 'shared-mime-info' 'desktop-file-utils' 'libxrender' 'libxtst' 'fontconfig' 'xdg-utils' 'qt5-tools')
install="vstax.install"
changelog="ChangeLog"
source=("https://sftp.vs.ch/${pkgname}/${_year}/${pkgname}${pkgver}-${pkgrel}_amd64.deb"
        "archlinux.patch" "vstax2024")
sha256sums=('bcd024715cd4180b8f578f114c544e1e988a3148aa2e9529a4889ccf62d4ef6c'
            'e27b65e54bd0e8be3bcaf09bfee76eb167de0640abd2d77713d87b5bb01907bc'
            '37ae2f14b4e7044c4264b0b8a1228562a9dea8c44c70a762747cc43ed43968ed')


prepare() {
    cd "${srcdir}/"

    # Extract the data contained in the `deb` file
    tar -xf data.tar.zst

    # Patch for archlinux
    patch -p2 < archlinux.patch
}

package() {
    cd "${srcdir}/usr/share/"

    # Store application name in a variable
    _appname="${pkgname}${_year}"

    # Copy the application descriptor
    install -D "applications/${_appname}.desktop" "${pkgdir}/usr/share/applications/${_appname}.desktop"

    # Copy the application changelog
    install -D "doc/${_appname}/changelog.Debian.gz" "${pkgdir}/usr/share/doc/${_appname}/changelog.Debian.gz"

    # Copy the mime descriptor
    install -D "mime/packages/${_appname}.xml" "${pkgdir}/usr/share/mime/packages/${_appname}.xml"

    # Copy the application itself
    install -d "${pkgdir}/usr/share/java/"
    cp -R "${_appname}" "${pkgdir}/usr/share/java/${_appname}"

    # Install our custom launcher script
    install -Dm755 "${srcdir}/vstax2024" "${pkgdir}/usr/bin/${_appname}"

    # Remove the included java environment
    rm -Rf "${pkgdir}/usr/share/java/${_appname}/jre"
}
