# Created by: Horst Leitenmueller <horst.leitenmueller@liwest.at>
# $FreeBSD$

PORTNAME=	openemm2015
PORTVERSION=	0
CATEGORIES=	mail java www
MASTER_SITES=	SF/openemm/OpenEMM%20software/OpenEMM%202015/ \
		http://central.maven.org/maven2/com/lowagie/itext/2.1.7/:MAVEN2
DISTNAME=	OpenEMM-2015-src
DISTFILES=	OpenEMM-2015-src.tar.gz OpenEMM-2015-JARs.tar.gz \
		itext-2.1.7.jar:MAVEN2

MAINTAINER=	horst.leitenmueller@liwest.at
COMMENT=	OpenEMM a email marketing enterprise app

LICENSE=	CPAL-1.0
LICENSE_FILE=	${WRKSRC}/other/LICENSE.txt

BROKEN_powerpc64=	fails to build: gmake[1]: clang: Command not found

LIB_DEPENDS=	libslang.so:devel/libslang2 \
		libsa.so:net/ossp-sa
BUILD_DEPENDS=	${TOMCATHOME}/bin/bootstrap.jar:www/tomcat8 \
		${ANT_CMD}:devel/apache-ant
RUN_DEPENDS=	${TOMCATHOME}/bin/bootstrap.jar:www/tomcat8 \
		py27-MySQLdb>=1.2.3:databases/py-MySQLdb

USES=	dos2unix gmake python:2.7,run
USE_RC_SUBR=	openemm2015
USE_JAVA=	yes
JAVA_VERSION=	1.7+

DOS2UNIX_GLOB=	*.c *.cpp *.h *.java *.xml *.properties
CC=	clang
CXX=	clang++

TOMCATVER=	apache-tomcat-8.0
TOMCATDIR=	${PREFIX}/${TOMCATVER}
TOMCATHOME=	${LOCALBASE}/${TOMCATVER}
#WEBAPPDIR=	${TOMCATDIR}/webapps
WEBAPP=		openemm

USER=		www
GROUP=		www
VAR_DIR?=	/var
LOG_DIR?=	${VAR_DIR}/log/${PORTNAME}
PID_FILE=	${VAR_DIR}/run/${PORTNAME}.pid

SUB_FILES=	pkg-message
SUB_LIST=	APP_SHORTNAME=${PORTNAME} \
		LOG_DIR=${LOG_DIR} \
		USER=${USER} \
		GROUP=${GROUP} \
		PID_FILE=${PID_FILE} \
		APP_HOME=${PREFIX}/${PORTNAME} \
		PREFIX=${PREFIX}
PLIST_SUB=	APP_HOME=${PORTNAME}

TOMCAT_CONFIG_FILES=	context.xml logging.properties server.xml

ANT_CMD?=	${LOCALBASE}/bin/ant
ANT=		${SETENV} JAVA_HOME=${JAVA_HOME} ${ANT_CMD}
ANT_TARGETS=	build
ANT_BUILD_PARAM=	-Dbuild=${WRKSRC}/target -Doe.home=${WRKSRC}/target -Dbin=${WRKSRC}/target/bin -Dcatalina.home=${TOMCATDIR} -Dsrc=${WRKSRC} -f openemm_build.xml
JAVA_BUILD=	yes

do-extract:
	@cd ${WRKDIR} && ${EXTRACT_CMD} ${EXTRACT_BEFORE_ARGS} ${_DISTDIR}/${DISTNAME}.tar.gz ${EXTRACT_AFTER_ARGS}

	# we extract the add-on libs to the ${WRKSRC}/lib directory
	${MKDIR} ${WRKSRC}/lib
	@cd ${WRKSRC}/lib && ${EXTRACT_CMD} ${EXTRACT_BEFORE_ARGS} ${_DISTDIR}/OpenEMM-2015-JARs.tar.gz ${EXTRACT_AFTER_ARGS}
	@cd ${WRKSRC}/lib && ${CP} ${_DISTDIR}/itext-2.1.7.jar .

post-extract:
	# remove not needed java libs which are installed as separate port
	${RM} ${WRKSRC}/lib/mysql*.jar

	${RM} ${WRKSRC}/backend/GNUMakefile
	# remove the c libs which are also installed as dependencies ports
	# ${WRKSRC}/backend/contrib/sa-1.2.6.tar.gz  -> net/ossp-sa   and ${WRKSRC}/backend/contrib/slang-1.4.9.tar.gz -> devel/libslang2
	${RM} -r ${WRKSRC}/backend/contrib

pre-patch:
	@${ECHO_MSG} "Installation settings:"
	@${ECHO_MSG} "   Location of TomcatHome:    ${TOMCAT_HOME}"
	@${ECHO_MSG} "   Location of JDK:          ${JAVA_HOME}"
	@${ECHO_MSG} "   Location of Java port:    ${JAVA_PORT}"

post-patch:
	${REINPLACE_CMD} -e 's|\/home\/openemm|${PREFIX}\/${PORTNAME}|g' -e 's|\/usr\/share\/doc\/OpenEMM-2015|${DOCSDIR}|g' ${WRKSRC}/other/README.txt
	${REINPLACE_CMD} -e 's|\/home\/openemm|${PREFIX}\/${PORTNAME}|g' ${WRKSRC}/backend/src/script/data/bav.rc
	${REINPLACE_CMD} -e 's|\/home\/openemm|${PREFIX}\/${PORTNAME}|g' ${WRKSRC}/tomcat/conf/server.xml

	${REINPLACE_CMD} -e 's|#!/usr/bin/env python|#!${PYTHON_CMD}|' -e "s|base = os.environ\['HOME'\]| base = '${PREFIX}\/${PORTNAME}'|g" -e "s|lockpath = os.environ\['LOCK_HOME'\]|lockpath = '/var/run/openemm'|g" ${WRKSRC}/backend/src/script/lib/agn.py

	${REINPLACE_CMD} -e 's|#!/usr/bin/env python|#!${PYTHON_CMD}|' ${WRKSRC}/backend/src/script/process/slrtscn.py
	${REINPLACE_CMD} -e 's|#!/usr/bin/env python|#!${PYTHON_CMD}|' ${WRKSRC}/backend/src/script/process/semu.py
	${REINPLACE_CMD} -e 's|#!/usr/bin/env python|#!${PYTHON_CMD}|' ${WRKSRC}/backend/src/script/process/softbounce.py
	${REINPLACE_CMD} -e 's|#!/usr/bin/env python|#!${PYTHON_CMD}|' ${WRKSRC}/backend/src/script/process/pickdist.py
	${REINPLACE_CMD} -e 's|#!/usr/bin/env python|#!${PYTHON_CMD}|' ${WRKSRC}/backend/src/script/process/smenable.py
	${REINPLACE_CMD} -e 's|#!/usr/bin/env python|#!${PYTHON_CMD}|' ${WRKSRC}/backend/src/script/process/bavd.py
	${REINPLACE_CMD} -e 's|#!/usr/bin/env python|#!${PYTHON_CMD}|' ${WRKSRC}/backend/src/script/process/bav-update.py
	${REINPLACE_CMD} -e 's|#!/usr/bin/env python|#!${PYTHON_CMD}|' ${WRKSRC}/backend/src/script/process/bav-trigger.py
	${REINPLACE_CMD} -e 's|#!/usr/bin/env python|#!${PYTHON_CMD}|' ${WRKSRC}/backend/src/script/process/upgrade.py
	${REINPLACE_CMD} -e 's|#!/usr/bin/env python|#!${PYTHON_CMD}|' ${WRKSRC}/backend/src/script/process/update.py
	${REINPLACE_CMD} -e 's|#!/usr/bin/env python|#!${PYTHON_CMD}|' ${WRKSRC}/backend/src/script/process/recovery.py

	${REINPLACE_CMD} -e 's|#!/usr/bin/env python|#!${PYTHON_CMD}|g' ${WRKSRC}/backend/src/script/lib/aps.readme

	${REINPLACE_CMD} -e 's|$$HOME|\${PREFIX}\/\${PORTNAME}|g' -e 's|python -c|${PYTHON_CMD} -c|g' ${WRKSRC}/backend/src/script/lib/config.sh

	${REINPLACE_CMD} -e 's|$$HOME|\${PREFIX}\/\${PORTNAME}|g' -e 's|-L INFO|-L INFO -s /var/run/openemm/bav.sock|' ${WRKSRC}/backend/src/script/control/bounce.sh
	${REINPLACE_CMD} -e 's|$$HOME|\${PREFIX}\/\${PORTNAME}|g' ${WRKSRC}/backend/src/script/control/bav-update.sh
	${REINPLACE_CMD} -e 's|$$HOME|\${PREFIX}\/\${PORTNAME}|g' ${WRKSRC}/backend/src/script/control/bav-trigger.sh
	${REINPLACE_CMD} -e 's|$$HOME|\${PREFIX}\/\${PORTNAME}|g' ${WRKSRC}/backend/src/script/control/bavd.sh

	${REINPLACE_CMD} -e 's|$$HOME|\${PREFIX}\/\${PORTNAME}|g' ${WRKSRC}/backend/src/script/control/merger.sh
	#${REINPLACE_CMD} -e 's|$$HOME|\${PREFIX}\/\${PORTNAME}|g' ${WRKSRC}/backend/src/script/control/openemm.py
	${REINPLACE_CMD} -e 's|$$HOME|\${PREFIX}\/\${PORTNAME}|g' ${WRKSRC}/backend/src/script/control/recovery.sh
	${REINPLACE_CMD} -e 's|$$HOME|\${PREFIX}\/\${PORTNAME}|g' ${WRKSRC}/backend/src/script/control/update.sh
	${REINPLACE_CMD} -e 's|$$HOME|\${PREFIX}\/\${PORTNAME}|g' ${WRKSRC}/backend/src/script/control/pickdist.sh

	${REINPLACE_CMD} -e 's|$$HOME|\${PREFIX}\/\${PORTNAME}|g' ${WRKSRC}/backend/src/script/control/mailer.sh
	${REINPLACE_CMD} -e 's|$$HOME|\${PREFIX}\/\${PORTNAME}|g' ${WRKSRC}/backend/src/script/control/sendmail-enable.sh

	${REINPLACE_CMD} -e 's|$$HOME|\${PREFIX}\/\${PORTNAME}|g' ${WRKSRC}/backend/src/script/control/slrtscn.sh
	${REINPLACE_CMD} -e 's|$$HOME|\${PREFIX}\/\${PORTNAME}|g' ${WRKSRC}/backend/src/script/control/softbounce.sh

	${REINPLACE_CMD} -e 's|=logs\/openemm\/|=\/var\/log\/\${PORTNAME}\/|g' ${WRKSRC}/src/conf/log4j.properties
	${REINPLACE_CMD} -e 's|=logs\/openemm-ws\/|=\/var\/log\/\${PORTNAME}\/|g' ${WRKSRC}/src/conf-ws/log4j.properties

	${REINPLACE_CMD} -e 's|system.logdir=.*|system.logdir=\/var\/log\/\${PORTNAME}\/|g' \
		-e 's|plugins.home=.*|plugins.home=\${PREFIX}\/\${PORTNAME}\/plugins|g' \
		-e 's|\/home\/openemm|${PREFIX}\/${PORTNAME}|g' ${WRKSRC}/src/conf/emm.properties

do-build:

	# build the c backend parts of AGN
	@${ECHO} "Building C Backend"

	cd ${WRKSRC}/backend && ${SETENV} ${MAKE_ENV}  ${MAKE_CMD}

	${MKDIR} ${WRKSRC}/target

	# copy some of the compiled c files to the bin
	${MKDIR} ${WRKSRC}/target/bin

	${MV} ${WRKSRC}/backend/src/c/bav/bav ${WRKSRC}/target/bin/
	${MV} ${WRKSRC}/backend/src/c/tools/qctrl ${WRKSRC}/target/bin/
	${MV} ${WRKSRC}/backend/src/c/tools/smctrl ${WRKSRC}/target/bin/
	${MV} ${WRKSRC}/backend/src/c/tools/updater ${WRKSRC}/target/bin/
	${MV} ${WRKSRC}/backend/src/c/xmlback/xmlback ${WRKSRC}/target/bin/

	# copy the script parts
	${MKDIR} ${WRKSRC}/target/bin/scripts

	${MV} ${WRKSRC}/backend/src/c/bav/bavwrap ${WRKSRC}/target/bin/scripts/

	# create the other directory parts
	${MKDIR} ${WRKSRC}/target/logs
	${MKDIR} ${WRKSRC}/target/logs/openemm
	${MKDIR} ${WRKSRC}/target/logs/openemm-ws
	${MKDIR} ${WRKSRC}/target/plugins
	${MKDIR} ${WRKSRC}/target/temp
	${MKDIR} ${WRKSRC}/target/webservices

	# build the java part, frontent/parts of backend for this port
	cd ${WRKSRC} && ${ANT} ${ANT_BUILD_PARAM} ${ANT_TARGETS}

	# cp the conf files
	${MKDIR} ${WRKSRC}/target/conf
.for config_file in ${TOMCAT_CONFIG_FILES}
	# rename the default template config files
	${MV} ${WRKSRC}/tomcat/conf/${config_file} ${WRKSRC}/target/conf/${config_file}.sample
.endfor
	# move the startup scripts
	${MV} ${WRKSRC}/backend/src/script/control/* ${WRKSRC}/target/bin/

	# clean bin directory
	${RM} ${WRKSRC}/target/bin/*.bat
	${RM} ${WRKSRC}/target/bin/openemm.py

	${MV} ${WRKSRC}/target/bin/USR_SHARE ${WRKSRC}/USR_SHARE
	${RM} -r ${WRKSRC}/target/src

	# remove problem tld with missing classes
	# SEEMS TO BE REMOVED ... @${RM} ${WRKSRC}/target/webapps/openemm/WEB-INF/struts-tiles.tld

do-install:
	${MKDIR} ${STAGEDIR}${PREFIX}/${PORTNAME}
	(cd ${WRKSRC}/target/ && ${COPYTREE_SHARE} \* ${STAGEDIR}${PREFIX}/${PORTNAME} "! -name USR_SHARE*")

	(cd ${STAGEDIR}${PREFIX}/${PORTNAME}/bin/scripts/ && \
		${LN} -sf bavwrap filter_or_forward && \
		${LN} -sf bavwrap is_no_systemmail && \
		${LN} -sf bavwrap scan_and_unsubscribe)

	(cd ${STAGEDIR}${PREFIX}/${PORTNAME}/webapps/ && \
		${LN} -sf openemm/assets openemmassets)

	# as needed from Running Q/A tests (stage-qa)
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/${PORTNAME}/bin/bav
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/${PORTNAME}/bin/smctrl
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/${PORTNAME}/bin/qctrl
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/${PORTNAME}/bin/xmlback
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/${PORTNAME}/bin/updater
	${STRIP_CMD} ${STAGEDIR}${PREFIX}/${PORTNAME}/bin/scripts/bavwrap

	# update some rights which must be assigned to root
	# we need the rigt to read the maillog file  /etc/newsyslog.conf
	# /var/log/maillog			640  7	   *	@T00  JC
	# and the actual file chmod 644 /var/log/maillog
	# if not change tracking of these mail sending / bounces is
	# not working correct

	# TODO LEI
	# move the licenses to the correct place
	# /usr/local/share/doc/openemm2013/LICENSE.txt
	# /usr/local/share/doc/openemm2013/ThirdPartyLicences/*

	# update also /usr/local/share with the docs
	${MKDIR} ${STAGEDIR}${DOCSDIR}
	(cd ${WRKSRC}/USR_SHARE/ && ${COPYTREE_SHARE} . ${STAGEDIR}${DOCSDIR})

	# create log files
	${MKDIR} ${STAGEDIR}/var/log/${PORTNAME}
	${TOUCH} ${STAGEDIR}/var/log/${PORTNAME}/openemm_core.log
	${TOUCH} ${STAGEDIR}/var/log/${PORTNAME}/userlogs.log
	${TOUCH} ${STAGEDIR}/var/log/${PORTNAME}/openemm_axis.log
	${TOUCH} ${STAGEDIR}/var/log/${PORTNAME}/daily.log

.include <bsd.port.mk>
