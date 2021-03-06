# Created by: Eric Anholt <anholt@FreeBSD.org>
# $FreeBSD$

PORTNAME=	mesa-dri
PORTVERSION=	${MESAVERSION}
CATEGORIES=	graphics

COMMENT=	OpenGL hardware acceleration drivers for DRI2+

USE_XORG=	dri2proto dri3proto glproto presentproto x11 xcb xdamage xext \
		xfixes xshmfence xv xvmc xxf86vm

OPTIONS_DEFINE=	TEXTURE VAAPI VDPAU
OPTIONS_DEFAULT=TEXTURE
OPTIONS_SUB=	yes

TEXTURE_DESC=	Enable texture-float support (patent encumbered)
TEXTURE_CONFIGURE_ENABLE=	texture-float

VAAPI_CONFIGURE_ENABLE=	va
VAAPI_LIB_DEPENDS=	libva.so:multimedia/libva

VDPAU_CONFIGURE_ENABLE=	vdpau
VDPAU_LIB_DEPENDS=	libvdpau.so:multimedia/libvdpau

.include <bsd.port.options.mk>
.include "${.CURDIR:H:H}/graphics/mesa-dri/Makefile.common"

ALL_DRI_DRIVERS=	I915 I965 RADEON R200 SWRAST
ALL_GALLIUM_DRIVERS=	FREEDRENO R300 R600 RADEONSI SVGA SWRAST VC4

DRI_DRIVERS=		SWRAST	# classic swrast
.if "${MESA_LLVM_VER}" != ""
GALLIUM_DRIVERS=	SWRAST	# llvmpipe
.else
GALLIUM_DRIVERS=	""
.endif
.if ${ARCH} == amd64 || ${ARCH} == i386 \
 || ${ARCH} == powerpc || ${ARCH} == powerpc64
DRI_DRIVERS+=		RADEON R200
GALLIUM_DRIVERS+=	R300 R600
. if "${MESA_LLVM_VER}" != ""	# until PPC gets LLVM in base
GALLIUM_DRIVERS+=	RADEONSI
. endif
.endif
.if ${ARCH} == amd64 || ${ARCH} == i386
DRI_DRIVERS+=		I915 I965
GALLIUM_DRIVERS+=	SVGA
. if ${OPSYS} == FreeBSD && ${OSVERSION} < 1100000
EXTRA_PATCHES+=	${PATCHDIR}/extra-src_mesa_drivers_dri_i965_intel__screen.c
. endif
.elif ${ARCH} == aarch64 || ${ARCH} == armv6 || ${ARCH} == armv7
GALLIUM_DRIVERS+=	FREEDRENO VC4
. if ${ARCH} == armv6 || ${ARCH} == armv7
EXTRA_PATCHES+=	${PATCHDIR}/extra-src_gallium_drivers_vc4_Makefile.in
. endif
.endif

CONFIGURE_ARGS+=	--with-dri-drivers="${DRI_DRIVERS:tl}" \
			--with-gallium-drivers="${GALLIUM_DRIVERS:tl}"

.for _d in ${ALL_DRI_DRIVERS}
. if ${DRI_DRIVERS:M${_d}}
PLIST_SUB+=	${_d}_DRIVER=""
. else
PLIST_SUB+=	${_d}_DRIVER="@comment "
. endif
.endfor

.for _gd in ${ALL_GALLIUM_DRIVERS}
. if ${GALLIUM_DRIVERS:M${_gd}}
PLIST_SUB+=	${_gd}_GDRIVER=""
. else
PLIST_SUB+=	${_gd}_GDRIVER="@comment "
. endif
.endfor

.include "${MASTERDIR}/Makefile.targets"

post-install:
	@${RM} -r ${STAGEDIR}/etc/OpenCL

.include <bsd.port.post.mk>
