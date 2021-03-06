# $FreeBSD$

PORTNAME=	rmilter
PORTVERSION=	1.10.0
CATEGORIES=	mail

MAINTAINER=	vsevolod@FreeBSD.org
COMMENT=	Milter that performs rspamd, clamav, and other checks

LICENSE=	BSD2CLAUSE

LIB_DEPENDS=	libpcre.so:devel/pcre

BROKEN_armv6=		Fails to configure in cmake
BROKEN_mips=		Fails to compile: error: Unsupported word size
BROKEN_sparc64=		Fails to compile: error: Unsupported word size
BROKEN_FreeBSD_9=	Fails to compile: error: Unsupported word size

USE_GITHUB=	yes
GH_ACCOUNT=	vstakhov

USES=		cmake:outsource bison pkgconfig ssl
USE_GNOME=	glib20

USERS=		_rmilter
GROUPS=		_rmilter

WITH_SENDMAIL_BASE=	yes

OPTIONS_DEFINE=		DKIM
OPTIONS_DEFAULT=	DKIM

DKIM_DESC=		DKIM signing support
DKIM_LIB_DEPENDS=	libopendkim.so:mail/opendkim

.include <bsd.port.pre.mk>

# Workaround to prevent bsd.milter.mk from breaking the build with the recent clang
WITHOUT_MILTER_LDFLAGS=	yes
.include "${PORTSDIR}/mail/sendmail/bsd.milter.mk"

.if ! ${PORT_OPTIONS:MDKIM}
CMAKE_ARGS+=	-DENABLE_DKIM=OFF
.endif

post-patch:
	${SED} ${SUB_LIST:S/$/!g/:S/^/ -e s!%%/:S/=/%%!/} \
		${WRKSRC}/freebsd/rmilter > ${WRKDIR}/rmilter

do-install:
	${INSTALL_SCRIPT} ${WRKDIR}/rmilter ${STAGEDIR}${PREFIX}/etc/rc.d/
	${INSTALL_PROGRAM} ${BUILD_WRKSRC}/rmilter ${STAGEDIR}${PREFIX}/sbin
	${INSTALL_DATA} ${WRKSRC}/rmilter.conf.sample ${STAGEDIR}${PREFIX}/etc/

post-stage:
	${INSTALL} -d ${STAGEDIR}/var/run/rmilter
.include <bsd.port.post.mk>
