# Created by: ports@c0decafe.net
# $FreeBSD$

PORTNAME=	App-perlbrew
PORTVERSION=	0.84
CATEGORIES=	devel perl5
MASTER_SITES=	CPAN
MASTER_SITE_SUBDIR=	CPAN:GUGOD
PKGNAMEPREFIX=	p5-

MAINTAINER=	sunpoet@FreeBSD.org
COMMENT=	Perl Environment manager

LICENSE=	MIT
LICENSE_FILE=	${WRKSRC}/LICENSE

BUILD_DEPENDS=	p5-CPAN-Perl-Releases>=3.66:devel/p5-CPAN-Perl-Releases \
		p5-Capture-Tiny>=0.36:devel/p5-Capture-Tiny \
		p5-Devel-PatchPerl>=1.52:devel/p5-Devel-PatchPerl \
		p5-local-lib>=2.000014:devel/p5-local-lib
RUN_DEPENDS:=	${BUILD_DEPENDS}
TEST_DEPENDS=	p5-File-Which>=1.21:sysutils/p5-File-Which \
		p5-IO-All>=0.51:devel/p5-IO-All \
		p5-Path-Class>=0.33:devel/p5-Path-Class \
		p5-Test-Exception>=0.32:devel/p5-Test-Exception \
		p5-Test-NoWarnings>=1.04:devel/p5-Test-NoWarnings \
		p5-Test-Output>=1.03:devel/p5-Test-Output \
		p5-Test-Spec>=0.47:devel/p5-Test-Spec \
		p5-Test-TempDir-Tiny>=0.016:devel/p5-Test-TempDir-Tiny

NO_ARCH=	yes
USE_PERL5=	modbuildtiny
USES=		perl5

.include <bsd.port.pre.mk>

.if ${PERL_LEVEL} < 502400
BUILD_DEPENDS+=	p5-Pod-Usage>=1.68:devel/p5-Pod-Usage
RUN_DEPENDS+=	p5-Pod-Usage>=1.68:devel/p5-Pod-Usage
.endif

.include <bsd.port.post.mk>
