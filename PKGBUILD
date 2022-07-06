# _     _            _        _          _____
#| |__ | | __ _  ___| | _____| | ___   _|___ /
#| '_ \| |/ _` |/ __| |/ / __| |/ / | | | |_ \
#| |_) | | (_| | (__|   <\__ \   <| |_| |___) |
#|_.__/|_|\__,_|\___|_|\_\___/_|\_\\__, |____/
#                                  |___/

#Maintainer: blacksky3 <blacksky3@tuta.io> <https://github.com/blacksky3>
#Credits: Janusz Lewandowski <lew21@xtreeme.org>
#Credits: David McFarland <corngood@gmail.com>
#Credits: Andrew Shark <ashark @at@ linuxcomp.ru>


#Upstream pages
#https://aur.archlinux.org/pkgbase/amdgpu-pro-installer
#https://github.com/Ashark/archlinux-amdgpu-pro
#https://repo.radeon.com/amdgpu

major=22.20
minor=1438747
ubuntu_ver=22.04
amf_version=1.4.26

pkgbase=vulkan-amdgpu-pro
pkgname=(amf-amdgpu-pro vulkan-amdgpu-pro lib32-vulkan-amdgpu-pro)
pkgver=${major}_${minor}
pkgrel=1
arch=(x86_64)
url='https://repo.radeon.com/amdgpu'
license=(custom: multiple)
source=(https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/a/amf-amdgpu-pro/amf-amdgpu-pro_${amf_version}-${minor}~${ubuntu_ver}_amd64.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/liba/libamdenc-amdgpu-pro/libamdenc-amdgpu-pro_1.0-${minor}~${ubuntu_ver}_amd64.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/v/vulkan-amdgpu-pro/vulkan-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
        https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/v/vulkan-amdgpu-pro/vulkan-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb)

# extracts a debian package
# $1: deb file to extract
extract_deb(){
  local tmpdir="$(basename "${1%.deb}")"
  rm -Rf "$tmpdir"
  mkdir "$tmpdir"
  cd "$tmpdir"
  ar x "$1"
  tar -C "${pkgdir}" -xf data.tar.xz
}

# move ubuntu specific /usr/lib/x86_64-linux-gnu to /usr/lib
# $1: debian package library dir (goes from opt/amdgpu or opt/amdgpu-pro and from x86_64 or i386)
# $2: arch package library dir (goes to usr/lib or usr/lib32)
move_libdir(){
  local deb_libdir="$1"
  local arch_libdir="$2"

  if [ -d "${pkgdir}/${deb_libdir}" ]; then
    if [ ! -d "${pkgdir}/${arch_libdir}" ]; then
      mkdir -p "${pkgdir}/${arch_libdir}"
    fi
    mv -t "${pkgdir}/${arch_libdir}/" "${pkgdir}/${deb_libdir}"/*
    find ${pkgdir} -type d -empty -delete
  fi
}

# move copyright file to proper place and remove debian changelog
move_copyright(){
  find ${pkgdir}/usr/share/doc -name "changelog.Debian.gz" -delete
  mkdir -p ${pkgdir}/usr/share/licenses/${pkgname}
  find ${pkgdir}/usr/share/doc -name "copyright" -exec mv {} ${pkgdir}/usr/share/licenses/${pkgname} \;
  find ${pkgdir}/usr/share/doc -type d -empty -delete
}

package_amf-amdgpu-pro(){
  pkgdesc='AMDGPU Pro Advanced Multimedia Framework'
  license=(custom: AMDGPU-PRO EULA)
  depends=(libdrm vulkan-amdgpu-pro=${major}_${minor}-${pkgrel})
  optdepends=('rocm-opencl-runtime: Warning unspecified optdep description')

  extract_deb "${srcdir}"/amf-amdgpu-pro_${amf_version}-${minor}~${ubuntu_ver}_amd64.deb
  extract_deb "${srcdir}"/libamdenc-amdgpu-pro_1.0-${minor}~${ubuntu_ver}_amd64.deb
  move_libdir "opt/amdgpu-pro/lib/x86_64-linux-gnu" "usr/lib"
  move_copyright
}

package_vulkan-amdgpu-pro(){
  pkgdesc='AMDGPU Pro Vulkan driver'
  license=(custom: AMDGPU-PRO EULA)
  provides=(vulkan-driver)
  depends=(vulkan-icd-loader)
  optdepends=('openssl: Warning unspecified optdep description')

  extract_deb "${srcdir}"/vulkan-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb
  move_libdir "opt/amdgpu-pro/lib/x86_64-linux-gnu" "usr/lib"
  move_copyright

  # extra_commands:
  mkdir -p "${pkgdir}"/usr/share/vulkan/icd.d/
  mv "${pkgdir}"/opt/amdgpu-pro/etc/vulkan/icd.d/amd_icd64.json "${pkgdir}"/usr/share/vulkan/icd.d/amd_pro_icd64.json
  mv "${pkgdir}"/usr/lib/amdvlk64.so "${pkgdir}"/usr/lib/amdvlkpro64.so
  sed -i "s#/opt/amdgpu-pro/lib/x86_64-linux-gnu/amdvlk64.so#/usr/lib/amdvlkpro64.so#" "${pkgdir}"/usr/share/vulkan/icd.d/amd_pro_icd64.json
  find ${pkgdir} -type d -empty -delete
}

package_lib32-vulkan-amdgpu-pro(){
  pkgdesc='AMDGPU Pro Vulkan driver (32-bit)'
  license=(custom: AMDGPU-PRO EULA)
  provides=(lib32-vulkan-driver)
  depends=(lib32-vulkan-icd-loader)
  optdepends=('lib32-openssl: Warning unspecified optdep description')

  extract_deb "${srcdir}"/vulkan-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
  move_libdir "opt/amdgpu-pro/lib/i386-linux-gnu" "usr/lib32"
  move_copyright

  # extra_commands:
  mkdir -p "${pkgdir}"/usr/share/vulkan/icd.d/
  mv "${pkgdir}"/opt/amdgpu-pro/etc/vulkan/icd.d/amd_icd32.json "${pkgdir}"/usr/share/vulkan/icd.d/amd_pro_icd32.json
  mv "${pkgdir}"/usr/lib32/amdvlk32.so "${pkgdir}"/usr/lib32/amdvlkpro32.so
  sed -i "s#/opt/amdgpu-pro/lib/i386-linux-gnu/amdvlk32.so#/usr/lib32/amdvlkpro32.so#" "${pkgdir}"/usr/share/vulkan/icd.d/amd_pro_icd32.json
  find ${pkgdir} -type d -empty -delete
}

sha256sums=('b8a936bcb181ca02df12cbb67773442955834b5443e7a89618774c5771185c2a'
            '1ad9a9a260c544d8f5f12fad51882c33f8a03f26482578ffab857d53d2f4fddb'
            '04145f457bc6e1491bc63485041926afb5b2b2290493d9dda9c026ab0c89d713'
            '8030f2702805bf42192e34cb2d7beb647aec2afa2106e87229a047855d2429f7')
