# $FreeBSD$

PORTNAME=	Cura
DISTVERSION=	4.8
PORTREVISION=	1
PORTEPOCH=	2
CATEGORIES=	cad
DIST_SUBDIR=	Ultimaker

MAINTAINER=	db@FreeBSD.org
COMMENT=	Slicing application for 3D printers

LICENSE=	AGPLv3
LICENSE_FILE=	${WRKSRC}/LICENSE

BUILD_DEPENDS=	Uranium>0,1:cad/uranium
RUN_DEPENDS=	${PYTHON_PKGNAMEPREFIX}zeroconf>0:net/py-zeroconf@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}pyserial>0:comms/py-pyserial@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}Shapely>0:devel/py-shapely@${PY_FLAVOR} \
		${PYTHON_PKGNAMEPREFIX}numpy-stl>0:math/py-numpy-stl@${PY_FLAVOR} \
		qt5-graphicaleffects>0:graphics/qt5-graphicaleffects \
		qt5-quickcontrols>0:x11-toolkits/qt5-quickcontrols \
		qt5-quickcontrols2>0:x11-toolkits/qt5-quickcontrols2 \
		Uranium>0:cad/uranium \
		libCharon>0:net/libcharon \
		CuraEngine>0,1:cad/cura-engine \
		fdm_materials>0,1:cad/fdm_materials \
		${PYTHON_PKGNAMEPREFIX}trimesh>0:devel/py-trimesh@${PY_FLAVOR}
LIB_DEPENDS=	libArcus.so:net/libarcus \
		libSavitar.so:devel/libsavitar

USES=		cmake gettext shebangfix python:3.5+ pyqt:5 \
		desktop-file-utils shared-mime-info
USE_PYQT=	gui widgets qml
CMAKE_ARGS+=	-DURANIUM_SCRIPTS_DIR:STRING="${LOCALBASE}/share/uranium/scripts"
USE_GITHUB=	yes
GH_ACCOUNT=	Ultimaker
GH_TAGNAME=	d115efbb
NO_ARCH=	yes

post-patch:
	${REINPLACE_CMD} -e 's|%%PYTHON_CMD%%|${PYTHON_CMD}|' \
		${WRKSRC}/cura_app.py

.include <bsd.port.mk>
