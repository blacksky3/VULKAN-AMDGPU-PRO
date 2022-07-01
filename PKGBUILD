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

pkgbase=amdgpu-pro
pkgname=(amf-amdgpu-pro amdgpu-pro-libgl lib32-amdgpu-pro-libgl vulkan-amdgpu-pro lib32-vulkan-amdgpu-pro)
pkgver=${major}_${minor}
pkgrel=1
arch=(x86_64)
url='https://repo.radeon.com/amdgpu'
license=(custom: multiple)
groups=('Radeon_Software_for_Linux')
source=(progl
				progl.bash-completion
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/a/appprofiles-amdgpu-pro/libgl1-amdgpu-pro-appprofiles_${major}-${minor}~${ubuntu_ver}_all.deb
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/a/amf-amdgpu-pro/amf-amdgpu-pro_${amf_version}-${minor}~${ubuntu_ver}_amd64.deb
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/liba/libamdenc-amdgpu-pro/libamdenc-amdgpu-pro_1.0-${minor}~${ubuntu_ver}_amd64.deb
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libegl1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libegl1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libgl1-amdgpu-pro-dri_${major}-${minor}~${ubuntu_ver}_i386.deb
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libgl1-amdgpu-pro-dri_${major}-${minor}~${ubuntu_ver}_amd64.deb
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libgl1-amdgpu-pro-ext_${major}-${minor}~${ubuntu_ver}_i386.deb
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libgl1-amdgpu-pro-ext_${major}-${minor}~${ubuntu_ver}_amd64.deb
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libgl1-amdgpu-pro-glx_${major}-${minor}~${ubuntu_ver}_i386.deb
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libgl1-amdgpu-pro-glx_${major}-${minor}~${ubuntu_ver}_amd64.deb
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libglapi1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libglapi1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libgles2-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/o/opengl-amdgpu-pro/libgles2-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/v/vulkan-amdgpu-pro/vulkan-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
				https://repo.radeon.com/amdgpu/${major}/ubuntu/pool/proprietary/v/vulkan-amdgpu-pro/vulkan-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb)

# extracts a debian package
# $1: deb file to extract
extract_deb() {
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
move_libdir() {
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
move_copyright() {
    find ${pkgdir}/usr/share/doc -name "changelog.Debian.gz" -delete
    mkdir -p ${pkgdir}/usr/share/licenses/${pkgname}
    find ${pkgdir}/usr/share/doc -name "copyright" -exec mv {} ${pkgdir}/usr/share/licenses/${pkgname} \;
    find ${pkgdir}/usr/share/doc -type d -empty -delete
}

package_amf-amdgpu-pro () {
    pkgdesc="AMDGPU Pro Advanced Multimedia Framework"
    license=('custom: AMDGPU-PRO EULA')
    depends=("libdrm" "vulkan-amdgpu-pro=${major}_${minor}-${pkgrel}")
    optdepends=("rocm-opencl-runtime: Warning unspecified optdep description")

    extract_deb "${srcdir}"/amf-amdgpu-pro_${amf_version}-${minor}~${ubuntu_ver}_amd64.deb
    extract_deb "${srcdir}"/libamdenc-amdgpu-pro_1.0-${minor}~${ubuntu_ver}_amd64.deb
    move_libdir "opt/amdgpu-pro/lib/x86_64-linux-gnu" "usr/lib"
    move_copyright
}

package_amdgpu-pro-libgl () {
    pkgdesc="AMDGPU Pro OpenGL driver"
    license=('custom: AMDGPU-PRO EULA')
    provides=('libgl')
    depends=("libdrm" "libx11" "libxcb" "libxdamage" "libxext" "libxfixes" "libxxf86vm")
    backup=(etc/amd/amdapfxx.blb)

    extract_deb "${srcdir}"/libegl1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb
    extract_deb "${srcdir}"/libgl1-amdgpu-pro-appprofiles_${major}-${minor}~${ubuntu_ver}_all.deb
    extract_deb "${srcdir}"/libgl1-amdgpu-pro-dri_${major}-${minor}~${ubuntu_ver}_amd64.deb
    extract_deb "${srcdir}"/libgl1-amdgpu-pro-ext_${major}-${minor}~${ubuntu_ver}_amd64.deb
    extract_deb "${srcdir}"/libgl1-amdgpu-pro-glx_${major}-${minor}~${ubuntu_ver}_amd64.deb
    extract_deb "${srcdir}"/libglapi1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb
    extract_deb "${srcdir}"/libgles2-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_amd64.deb
    move_copyright

    # extra_commands:
    move_libdir "usr/lib/x86_64-linux-gnu" "usr/lib"
    move_libdir "opt/amdgpu-pro/lib/x86_64-linux-gnu" "usr/lib/amdgpu-pro"
    move_libdir "opt/amdgpu-pro/lib/xorg" "usr/lib/amdgpu-pro/xorg"
    move_libdir "opt/amdgpu/share/drirc.d" "usr/share/drirc.d"
    sed -i "s|/opt/amdgpu-pro/lib/x86_64-linux-gnu|#/usr/lib/amdgpu-pro  # commented to prevent problems of booting with amdgpu-pro, use progl script|" "${pkgdir}"/etc/ld.so.conf.d/10-amdgpu-pro-x86_64.conf
    install -Dm755 "${srcdir}"/progl "${pkgdir}"/usr/bin/progl
    install -Dm755 "${srcdir}"/progl.bash-completion "${pkgdir}"/usr/share/bash-completion/completions/progl
    # For some reason, applications started with normal OpenGL (i.e. without ag pro) crashes at launch if this conf file is presented, so hide it for now, until I find out the reason of that.
    mv "${pkgdir}"/usr/share/drirc.d/10-amdgpu-pro.conf "${pkgdir}"/usr/share/drirc.d/10-amdgpu-pro.conf.hide
}

package_lib32-amdgpu-pro-libgl () {
    pkgdesc="AMDGPU Pro OpenGL driver (32-bit)"
    license=('custom: AMDGPU-PRO EULA')
    provides=('lib32-libgl')
    depends=("amdgpu-pro-libgl=${major}_${minor}-${pkgrel}" "lib32-libdrm" "lib32-libx11" "lib32-libxcb" "lib32-libxdamage" "lib32-libxext" "lib32-libxfixes" "lib32-libxxf86vm")
    backup=(etc/amd/amdrc etc/ld.so.conf.d/10-amdgpu-pro-i386.conf)

    extract_deb "${srcdir}"/libegl1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
    extract_deb "${srcdir}"/libgl1-amdgpu-pro-dri_${major}-${minor}~${ubuntu_ver}_i386.deb
    extract_deb "${srcdir}"/libgl1-amdgpu-pro-ext_${major}-${minor}~${ubuntu_ver}_i386.deb
    extract_deb "${srcdir}"/libgl1-amdgpu-pro-glx_${major}-${minor}~${ubuntu_ver}_i386.deb
    extract_deb "${srcdir}"/libglapi1-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
    extract_deb "${srcdir}"/libgles2-amdgpu-pro_${major}-${minor}~${ubuntu_ver}_i386.deb
    move_copyright

    # extra_commands:
    rm "${pkgdir}"/etc/amd/amdrc "${pkgdir}"/opt/amdgpu-pro/lib/xorg/modules/extensions/libglx.so "${pkgdir}"/opt/amdgpu/share/drirc.d/10-amdgpu-pro.conf
    move_libdir "usr/lib/i386-linux-gnu" "usr/lib32"
    move_libdir "opt/amdgpu-pro/lib/i386-linux-gnu" "usr/lib32/amdgpu-pro"
    sed -i "s|/opt/amdgpu-pro/lib/i386-linux-gnu|#/usr/lib32/amdgpu-pro  # commented to prevent problems of booting with amdgpu-pro, use progl32 script|" "${pkgdir}"/etc/ld.so.conf.d/10-amdgpu-pro-i386.conf
}

package_vulkan-amdgpu-pro () {
    pkgdesc="AMDGPU Pro Vulkan driver"
    license=('custom: AMDGPU-PRO EULA')
    provides=('vulkan-driver')
    depends=("vulkan-icd-loader")
    optdepends=("openssl: Warning unspecified optdep description")

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

package_lib32-vulkan-amdgpu-pro () {
    pkgdesc="AMDGPU Pro Vulkan driver (32-bit)"
    license=('custom: AMDGPU-PRO EULA')
    provides=('lib32-vulkan-driver')
    depends=("lib32-vulkan-icd-loader")
    optdepends=("lib32-openssl: Warning unspecified optdep description")

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

sha256sums=('feb74796c3152cbafaba89d96e68a152f209bd3058c7eb0413cbe1ab0764e96f'
            'e32801c38b475cd8df17a407726b86db3de26410f563d688325b4d4314fc5354'
            '311afa87708a502d1a3e15662fffe6030bbebf511e709ae47393cc54852d95d8'
            'b8a936bcb181ca02df12cbb67773442955834b5443e7a89618774c5771185c2a'
            '1ad9a9a260c544d8f5f12fad51882c33f8a03f26482578ffab857d53d2f4fddb'
            '129721ead82e6a42a2c0a159983f340540c03fffddc6d01a73118bd303ac43aa'
            '78d6b94d802b78612602561fb0441af988fdff8b3b14a36c2c84d6ef25b48644'
            '71d7ba006a985b8b3cef1b19a9200789ff78e4ce0569b9efdd958f983034105a'
            '84a7c66ec82208bcacd22708d9507b040b6df4b3637743f779e711c69a654c80'
            'b6b70ad6d9b5572ff1c7cceafc5a3154255e8f95087f400cf20f6ce9b605ecd1'
            'ab94609fde3719b194d3a1ac1126fb8048124af1024171b216d0cd4294682539'
            '93931a97723ce6158d3ea9befdc7a6a452b015ce8467ef11f38997f90aa0c354'
            '3dd97653dc0e3536a3f16bf6c473aed3eff906eee39958d4c7dcd72c0ecd8db1'
            '2e4fe116c2895e3761798eba008f387b8c3f7690b8a80dd986016d505024367c'
            '6aa5b0cd23aacc5776f294312013b44b7e6ba5733739bd08e32bb272f9fdf33d'
            '94060c15ad3a1831a395b50008e6e0a59f5357cc3b71c367c1704fdc0c093d11'
            '57464c3cb471c65eb42ff4783eac86b5d3a3b509e78401b5e078ef95d33d2559'
            '04145f457bc6e1491bc63485041926afb5b2b2290493d9dda9c026ab0c89d713'
            '8030f2702805bf42192e34cb2d7beb647aec2afa2106e87229a047855d2429f7')
