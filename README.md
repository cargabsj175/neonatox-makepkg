  makepkg - Una herramienta bash pura para construir PKGBUILD 

neonatox makepkg
=======

![Bash](https://img.shields.io/badge/shell-bash-green.svg?style=flat-square) ![Free Software](https://img.shields.io/badge/license-Free%20Software-blue.svg?style=flat-square) ![Version n2026](https://img.shields.io/badge/version-n2026-blue.svg?style=flat-square) ![Inspired by Arch Linux makepkg](https://img.shields.io/badge/inspired_by-Arch%20Linux%20makepkg-1793D1.svg?style=flat-square&logo=arch-linux&logoColor=white) ![Unix-like](https://img.shields.io/badge/platform-Unix--like-orange.svg?style=flat-square)

**Versión:** n2026 (o la versión de nhopkg si se ejecuta dentro de nhopkg)

**Descripción:** Una herramienta bash pura para construir paquetes a partir de archivos PKGBUILD, imitando el comportamiento de makepkg de Arch Linux. Utiliza nhopkg para manejar dependencias y procesos de construcción. Soporta paquetes simples (no divididos) y está diseñado para ser ligero y compatible con entornos bash.

**Autor:** Carlos Sanchez <cargabsj175@gmail.com>

**Licencia:** Software libre; consulta el código fuente para condiciones de copia. NO HAY GARANTÍA, en la medida permitida por la ley.

Características
---------------

*   Descarga fuentes desde URLs (soporte para HTTP, HTTPS, Git, etc.).
*   Verificación de sumas de comprobación (sha256sums, md5sums, etc.).
*   Extracción de archivos comprimidos (tar.gz, tar.bz2, tar.xz, tar.zst, zip).
*   Ejecución de funciones PKGBUILD: prepare(), build(), package().
*   Generación de metadatos (.PKGINFO, .BUILDINFO, .MTREE).
*   Creación de paquetes .pkg.tar.zst (compatible con pacman).
*   Soporte para arquitecturas específicas (x86\_64, aarch64).
*   Generación de .SRCINFO para repositorios AUR-like.
*   Opciones para omitir verificaciones o no crear archivos de paquete.

Requisitos
----------

*   Bash 4+ (set -euo pipefail).
*   Herramientas básicas: curl, git, sha256sum, md5sum, bsdtar (opcional para .MTREE), zstd, unzip, tar.
*   nhopkg (opcional, pero recomendado para entornos integrados).

Uso
---

Ejecuta el script en un directorio que contenga un archivo PKGBUILD válido.

    ./makepkg [opciones]

### Opciones

*   `-D, --dir <dir>`: Cambia al directorio <dir> antes de procesar el PKGBUILD.
*   `--noarchive`: No crea el archivo de paquete (.pkg.tar.zst).
*   `--skipchecksums`: No verifica las sumas de comprobación de las fuentes descargadas.
*   `--printsrcinfo`: Imprime el .SRCINFO generado y sale.
*   `-h, --help`: Muestra este mensaje de ayuda y sale.
*   `-V, --version`: Muestra información de versión y sale.

Ejemplo de Flujo de Trabajo
---------------------------

1.  Clona o crea un directorio con PKGBUILD.
2.  Ejecuta `./makepkg`.
3.  El script descargará fuentes, verificará checksums, extraerá, ejecutará prepare/build/package, generará metadatos y creará el paquete.
4.  El paquete resultante estará en el directorio actual (ej: paquete-1.0-1-x86\_64.pkg.tar.zst).

Limitaciones
------------

*   Solo soporta PKGBUILD no divididos (una sola función package()).
*   No maneja paquetes divididos (split packages) ni dependencias complejas sin nhopkg.
*   Requiere herramientas externas para ciertas compresiones (zstd, bsdtar).
*   No instala paquetes; usa pacman o herramientas similares para eso.

Ejemplos de PKGBUILD Soportados
-------------------------------

Aquí hay ejemplos de PKGBUILD que funcionan con esta herramienta:

### Ejemplo Simple: rmg

    # Maintainer: Rosalie Wanders <rosalie@mailbox.org>
    pkgname=rmg
    pkgver=0.8.6
    pkgrel=1
    pkgdesc="Rosalie's Mupen GUI"
    arch=('x86_64' 'aarch64')
    url="https://github.com/Rosalie241/$pkgname"
    license=('GPL3')
    
    depends=("libusb" "hidapi" "libsamplerate" "speexdsp" "minizip" "sdl3" "zlib" "freetype2" "qt6-base" "qt6-svg" "qt6-websockets")
    makedepends=("git" "nasm" "cmake" "vulkan-headers")
    
    source=("git+https://github.com/Rosalie241/$pkgname.git#tag=v$pkgver")
    sha256sums=('SKIP')
    
    prepare() { ... }
    build() { ... }
    package() { ... }
    

### Ejemplo Binario: brave-bin

    # Maintainer: brave <aur-release@brave.com>
    pkgname=brave-bin
    pkgver=1.84.132
    pkgrel=1
    epoch=1
    pkgdesc='Web browser that blocks ads and trackers by default (binary release)'
    arch=(x86_64 aarch64)
    url=https://brave.com
    license=(MPL2 BSD custom:chromium)
    depends=(alsa-lib gtk3 libxss nss ttf-font)
    optdepends=('cups: Printer support' ...)
    
    source=($pkgname.sh brave-browser.desktop)
    source_x86_64=(${pkgname}-${pkgver}-x86_64.zip::https://github.com/brave/brave-browser/releases/download/v${pkgver}/brave-browser-${pkgver}-linux-amd64.zip)
    sha256sums=('...' ...)
    
    prepare() { ... }
    package() { ... }
    

Contribuciones y Reportes de Errores
------------------------------------

Envía pull requests o issues a través de GitHub o contacta al autor por email. ¡Contribuciones bienvenidas!

Copyright (C) 2007-2025 Carlos Sanchez <cargabsj175@gmail.com>.
