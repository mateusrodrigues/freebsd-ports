# Created by: torstenb
# $FreeBSD$

PORTNAME=	libwww
PORTVERSION=	5.4.2
CATEGORIES=	www devel
MASTER_SITES=	https://www.w3.org/Library/Distribution/ \
		https://www.w3.org/Library/Distribution/old/
DISTNAME=	w3c-${PORTNAME}-${PORTVERSION}

MAINTAINER=	dbaio@FreeBSD.org
COMMENT=	W3C Reference Library

LICENSE=	W3C
LICENSE_NAME=	W3C IPR SOFTWARE NOTICE
LICENSE_FILE=	${WRKSRC}/LICENSE.html
LICENSE_PERMS=	dist-mirror dist-sell pkg-mirror pkg-sell auto-accept

BROKEN_SSL=		openssl-devel

LIB_DEPENDS=	libexpat.so:textproc/expat2

USES=		gmake libtool localbase perl5 ssl tar:tgz
USE_PERL5=	build
USE_LDCONFIG=	yes
GNU_CONFIGURE=	yes
CONFIGURE_ARGS=	--enable-shared --with-zlib --with-ssl=${OPENSSLBASE}

post-install:
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/lib/*.so

.include <bsd.port.mk>
