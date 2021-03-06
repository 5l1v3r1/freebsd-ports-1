# $FreeBSD$

PORTNAME=	openjdk11
PORTVERSION=	11.0.5.10.1
CATEGORIES=	java devel
MASTER_SITES=	LOCAL/glewis/bootstrap-openjdk11 \
		LOCAL/pkubaj
PKGNAMEPREFIX=	bootstrap-
DISTNAME=	${JDK_PORT}-${JDK_ARCH}-${PORTVERSION}

MAINTAINER=	java@FreeBSD.org
COMMENT=	Java Development Kit 11

LICENSE=	GPLv2

ONLY_FOR_ARCHS=	amd64 i386 powerpc64 powerpc64le

USES=		tar:xz

NO_BUILD=	yes
PLIST_SUB=	JDK_ROOT=${JDK_ROOT}
WRKSRC=		${WRKDIR}/${JDK_ROOT}

JDK_PORT=	${PKGNAMEPREFIX}${PORTNAME}
JDK_ROOT=	${PKGNAMEPREFIX}${PORTNAME}
JDK_ARCH=	${ARCH}${JDK_ARCH_SUFFIX_${ARCH}}
JDK_ARCH_SUFFIX_powerpc64=	-${PPC_ABI:tl}

INSTALLDIR=	${STAGEDIR}${PREFIX}/${JDK_ROOT}

.include <bsd.port.pre.mk>

# Support aarch64 on FreeBSD 12 and up
.if ${OSVERSION} >= 1200500
ONLY_FOR_ARCHS+=aarch64
.endif

.if ${ARCH} == aarch64
DISTNAME=	${JDK_PORT}-${JDK_ARCH}-12-${PORTVERSION}
.elif ${ARCH} == powerpc64le
PLIST_SUB+=	PPC64LE="@comment "
.else
PLIST_SUB+=	PPC64LE=""
.endif

post-extract:
	-${MV} ${WRKSRC}/include/bsd ${WRKSRC}/include/freebsd

do-install:
	@cd ${WRKSRC} && ${COPYTREE_SHARE} . ${INSTALLDIR}
	@cd ${WRKSRC} && ${COPYTREE_BIN} bin ${INSTALLDIR}
	@${CHMOD} a+x ${INSTALLDIR}/lib/jspawnhelper

.include <bsd.port.post.mk>
