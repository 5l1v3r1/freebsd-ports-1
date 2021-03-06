# Created by: David O'Brien <obrien@FreeBSD.org>
# $FreeBSD$

PORTNAME=	zeek
PORTVERSION=	3.0.12
PORTREVISION=	2
CATEGORIES=	security
MASTER_SITES=	https://old.zeek.org/downloads/
DISTFILES=	${DISTNAME}${EXTRACT_SUFX}

MAINTAINER=	leres@FreeBSD.org
COMMENT=	System for detecting network intruders in real-time

LICENSE=	BSD3CLAUSE

BROKEN_powerpc64=	Does not build: error: zero-size array 'names'

USES=		bison cmake compiler:c++11-lang gettext-runtime ninja perl5 python shebangfix ssl
USE_LDCONFIG=	yes

PORTSCOUT=	limit:0,even

CMAKE_ARGS+=	-DPYTHON_EXECUTABLE:PATH=${PYTHON_CMD}
CXXFLAGS+=	-std=c++11 -Wall

CONFLICTS=	bro

SHEBANG_FILES=	aux/zeekctl/aux/trace-summary/trace-summary

SUB_FILES=	pkg-message
NO_MTREE=	yes

CMAKE_ARGS+=	-GNinja \
		-D CMAKE_INSTALL_PREFIX:PATH=${PREFIX} \
		-D ZEEK_ROOT_DIR:PATH=${PREFIX} \
		-D PY_MOD_INSTALL_DIR:PATH=${PREFIX}/lib/zeekctl \
		-D ZEEK_SCRIPT_INSTALL_PATH:PATH=${PREFIX}/share/zeek \
		-D ZEEK_ETC_INSTALL_DIR:PATH=${PREFIX}/etc \
		-D ZEEK_MAN_INSTALL_PATH=${MANPREFIX}/man \
		-D ENABLE_PERFTOOLS_DEBUG:BOOL=false \
		-D BinPAC_SKIP_INSTALL:BOOL=true \
		-D INSTALL_AUX_TOOLS:BOOL=true \
		-D BUILD_SHARED_LIBS:BOOL=true \
		-D BUILD_STATIC_BROKER:BOOL=true \
		-D CMAKE_EXE_LINKER_FLAGS="${OPENSSL_LDFLAGS}"

ZEEKUSER?=	zeek
ZEEKGROUP?=	zeek
PLIST_SUB+=	ARCH=${UNAME_M} \
		ZEEKUSER=${ZEEKUSER} \
		ZEEKGROUP=${ZEEKGROUP} \
		LCASE_OPSYS=${OPSYS:tl}

USERS=		${ZEEKUSER}
GROUPS=		${ZEEKGROUP}

OPTIONS_DEFINE= BROKER DEBUG GEOIP2 IPSUMDUMP LBL_CF LBL_HF NETMAP \
		PERFTOOLS ZEEKCTL
OPTIONS_DEFAULT=BROKER GEOIP2 IPSUMDUMP LBL_CF LBL_HF NETMAP ZEEKCTL
OPTIONS_SUB=	yes

BROKER_DESC=	Enable the Broker communication library
GEOIP2_DESC=	Build with GeoIP2 (MaxMindDB) support
IPSUMDUMP_DESC=	Enables traffic summaries
LBL_CF_DESC=	Unix time to formated time/date filter support
LBL_HF_DESC=	Address to hostname filter support
NETMAP_DESC=	Native Netmap Packet IOSource for Bro
PERFTOOLS_DESC=	Use Perftools to improve memory & CPU usage
ZEEKCTL_DESC=	ZeekControl support (implies BROKER and IPSUMDUMP)

ZEEKCTL_IMPLIES=		BROKER IPSUMDUMP

BROKER_BUILD_DEPENDS=	swig:devel/swig
BROKER_CMAKE_BOOL=	ENABLE_BROKER
DEBUG_CMAKE_BOOL=	ENABLE_DEBUG
GEOIP2_LIB_DEPENDS=	libmaxminddb.so:net/libmaxminddb
IPSUMDUMP_BUILD_DEPENDS=ipsumdump:net/ipsumdump
IPSUMDUMP_RUN_DEPENDS=	ipsumdump:net/ipsumdump
LBL_CF_RUN_DEPENDS=	${LOCALBASE}/bin/cf:sysutils/lbl-cf
LBL_HF_RUN_DEPENDS=	${LOCALBASE}/bin/hf:sysutils/lbl-hf
NETMAP_GH_TUPLE=	bro:bro-netmap:f3620df:bro_netmap
NETMAP_USE=		GITHUB=nodefault
PERFTOOLS_BUILD_DEPENDS=${LOCALBASE}/bin/perftools-pprof:devel/google-perftools
PERFTOOLS_CMAKE_BOOL=	ENABLE_PERFTOOLS
PERFTOOLS_RUN_DEPENDS=${LOCALBASE}/bin/perftools-pprof:devel/google-perftools
PYTHON_BUILD_DEPENDS=	swig:devel/swig
ZEEKCTL_BUILD_DEPENDS=	${LOCALBASE}/bin/bash:shells/bash \
			${PYTHON_PKGNAMEPREFIX}sqlite3>0:databases/py-sqlite3@${PY_FLAVOR} \
			swig:devel/swig
ZEEKCTL_CMAKE_BOOL=	INSTALL_ZEEKCTL
ZEEKCTL_RUN_DEPENDS=	${LOCALBASE}/bin/bash:shells/bash \
			${PYTHON_PKGNAMEPREFIX}sqlite3>0:databases/py-sqlite3@${PY_FLAVOR}

.include <bsd.port.options.mk>

.if ${PORT_OPTIONS:MZEEKCTL}
USE_RC_SUBR=	zeek
.endif

post-patch:
	# Silence the "use ZeekControl.plugin instead of BroControl.plugin" nag
	@${REINPLACE_CMD} -e 's/^print/#&/' \
	    ${WRKSRC}/aux/zeekctl/BroControl/__init__.py

post-install-ZEEKCTL-on:
	${MKDIR} ${STAGEDIR}${PREFIX}/logs
	${MKDIR} ${STAGEDIR}${PREFIX}/spool/tmp
	${MKDIR} ${STAGEDIR}${PREFIX}/spool/installed-scripts-do-not-touch/auto
	${MKDIR} ${STAGEDIR}${PREFIX}/spool/installed-scripts-do-not-touch/site
.for F in zeekctl.cfg networks.cfg node.cfg
	${MV} ${STAGEDIR}${PREFIX}/etc/${F} ${STAGEDIR}${PREFIX}/etc/${F}.sample
.endfor
	${RM} ${STAGEDIR}${PREFIX}/share/zeekctl/scripts/zeekctl-config.sh
	${LN} -s ../../../spool/zeekctl-config.sh \
	    ${STAGEDIR}${PREFIX}/share/zeekctl/scripts/zeekctl-config.sh

post-install:
	${MV} ${STAGEDIR}${DATADIR}/site/local.zeek \
	    ${STAGEDIR}${DATADIR}/site/local.zeek.sample

pre-install-ZEEKCTL-on:
	${MKDIR} ${STAGEDIR}${PREFIX}/etc/rc.d

post-build-NETMAP-on:
	${LN} -s ../.build ${WRKSRC}/build
	cd ${WRKSRC_bro_netmap} && ./configure --bro-dist=${WRKSRC} \
	    --install-root=${STAGEDIR}${PREFIX}/lib/zeek/plugins && \
	    make && make install

.include <bsd.port.pre.mk>

UNAME_M!=	${UNAME} -m

.include <bsd.port.post.mk>
