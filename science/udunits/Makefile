# Created by: Greg Lewis <glewis@FreeBSD.org>
# $FreeBSD$

PORTNAME=	udunits
PORTVERSION=	2.2.26
CATEGORIES=	science math
MASTER_SITES=	ftp://ftp.unidata.ucar.edu/pub/udunits/ \
		LOCAL/sunpoet

MAINTAINER=	sunpoet@FreeBSD.org
COMMENT=	Library for manipulating units of physical quantities

LICENSE=	UCAR_UNIDATA
LICENSE_FILE=	${WRKSRC}/COPYRIGHT
LICENSE_NAME=	University Corporation for Atmospheric Research/Unidata
LICENSE_PERMS=	dist-mirror dist-sell pkg-mirror pkg-sell auto-accept

LIB_DEPENDS=	libexpat.so:textproc/expat2

OPTIONS_DEFINE=	DOCS

CONFIGURE_ENV=	ac_cv_header_CUnit_CUnit_h=no
GNU_CONFIGURE=	yes
INSTALL_TARGET=	install-strip
TEST_TARGET=	check
USE_LDCONFIG=	yes
USES=		libtool localbase

INFO=		udunits2 udunits2lib udunits2prog

post-patch:
	@${REINPLACE_CMD} -e '/install-data-hook$$/d' ${WRKSRC}/lib/Makefile.in

.include <bsd.port.mk>
