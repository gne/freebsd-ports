# $FreeBSD$

PORTNAME=	dte
DISTVERSIONPREFIX=	v
DISTVERSION=	1.5
CATEGORIES=	editors

MAINTAINER=	dg@syrec.org
COMMENT=	Small and easy to use console text editor

LICENSE=	GPLv2
LICENSE_FILE=	${WRKSRC}/LICENSE

USES=		iconv gmake ncurses
MAKEFILE=	${WRKSRC}/GNUmakefile
MAKE_ARGS=	V=1 prefix=${PREFIX} mandir=${PREFIX}/man
USE_GITHUB=	yes
GH_ACCOUNT=	craigbarnes
TEST_TARGET=	check

PLIST_FILES=	bin/dte \
		man/man1/dte.1.gz \
		man/man5/dte-syntax.5.gz \
		man/man5/dterc.5.gz \

PORTDOCS=	README.md

OPTIONS_DEFINE=	DOCS

post-install:
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/bin/dte
	@${MKDIR} ${STAGEDIR}${DOCSDIR}
	${INSTALL_MAN} ${PORTDOCS:S|^|${WRKSRC}/|} ${STAGEDIR}${DOCSDIR}

.include <bsd.port.mk>
