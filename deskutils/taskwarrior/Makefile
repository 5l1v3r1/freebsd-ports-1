# Created by: gahr
# $FreeBSD$

PORTNAME=	taskwarrior
PORTVERSION=	2.5.1
CATEGORIES=	deskutils

MASTER_SITES=	https://taskwarrior.org/download/
DISTNAME=	task-${PORTVERSION}

MAINTAINER=	skreuzer@FreeBSD.org
COMMENT=	Feature-rich command-line todo list manager

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/LICENSE

LIB_DEPENDS=	libgnutls.so:security/gnutls
TEST_DEPENDS=	faketime:devel/libfaketime

USES=		compiler:c++11-lib cmake python:test shebangfix

CMAKE_ARGS+=	-DTASK_DOCDIR:STRING=${DATADIR:S/${PREFIX}\///} \
		-DTASK_MAN1DIR:STRING=man/man1 \
		-DTASK_MAN5DIR:STRING=man/man5

SHEBANG_FILES=	scripts/*/*.pl \
		doc/rc/refresh \

do-test:
	@cd ${WRKSRC}/test && \
	${PYTHON_CMD} run_all --verbose && \
	${PYTHON_CMD} problems

.include <bsd.port.mk>
