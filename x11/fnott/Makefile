# $FreeBSD$

PORTNAME=	fnott
DISTVERSION=	1.0.0
CATEGORIES=	x11
MASTER_SITES=	https://codeberg.org/dnkl/${PORTNAME}/archive/${DISTVERSIONFULL}${EXTRACT_SUFX}?dummy=/

PATCH_SITES=	https://github.com/jbeich/${PORTNAME}/commit/
PATCHFILES+=	3b2cc4b817a2.patch:-p1 # i386
PATCHFILES+=	a9c864a0ae05.patch:-p1 # Clang
PATCHFILES+=	b2bc271ff5b3.patch:-p1 # CLOCK_BOOTTIME
PATCHFILES+=	c27c03bb3e70.patch:-p1 # memfd_create
PATCHFILES+=	062ddc9612b8.patch:-p1 # _POSIX_C_SOURCE
PATCHFILES+=	a1a06b71a2ee.patch:-p1 # epoll-shim

MAINTAINER=	jbeich@FreeBSD.org
COMMENT=	Keyboard-driven notification daemon for wlroots-based compositors

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/LICENSE

BUILD_DEPENDS=	evdev-proto>0:devel/evdev-proto \
		tllist>=1.0.1:devel/tllist \
		wayland-protocols>0:graphics/wayland-protocols \
		scdoc:textproc/scdoc
LIB_DEPENDS=	libdbus-1.so:devel/dbus \
		libepoll-shim.so:devel/libepoll-shim \
		libpng.so:graphics/png \
		libwayland-client.so:graphics/wayland \
		libfcft.so:x11-fonts/fcft

USES=		compiler:c11 meson pkgconfig xorg
USE_XORG=	pixman
WRKSRC=		${WRKDIR}/${PORTNAME}

# XXX Drop after FreeBSD 12.1 EOL
.if exists(/usr/lib/clang/8.0.1)
# notification.c:545:13: error: suggest braces around initialization of subobject [-Werror,-Wmissing-braces]
CFLAGS+=	-Wno-error=missing-braces
.endif

post-patch:
	@${REINPLACE_CMD} 's,/usr/local,${PREFIX},g' \
		${WRKSRC}/xdg.c

.include <bsd.port.mk>
