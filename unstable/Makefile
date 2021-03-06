APP				:= unstable
CHROME_DIR		:= deploy-chrome
XR_DIR			:= xulrunner-wrapper
APP_DIR			:= ${XR_DIR}/${APP}
CONTENT			:= ${APP_DIR}/content/app/
XR_VERSION		:= 26.0

${APP_DIR}/xulrunner:
	@echo ""
	@echo "Select a platform by running one of the following before building:"
	@echo "   make chrome"
	@echo "   make linux"
	@echo "   make linux64"
	@echo "   make mac"
	@echo "   make win"
	@echo ""
	@false

unstable.appcache:
	@echo "CACHE MANIFEST"								>  unstable.appcache
	@echo "# `date`"									>> unstable.appcache
	@ls *.js *.css *.png						>> unstable.appcache
	@find ../src ../fonts ../images images	-type f		>> unstable.appcache

appcache: unstable.appcache

chrome:
	mkdir -p ${CHROME_DIR}/${APP}
	cp -r manifest.json icon*.png ../fonts ../src ${CHROME_DIR}/
	cp ../index.html ${CHROME_DIR}/
	cp -r *.html *.css *.js images ${CHROME_DIR}/${APP}
	(cd ${CHROME_DIR} && zip -r ../unstable-chrome.zip *)

linux: ${XR_DIR}/xulrunner-${XR_VERSION}.en-US.linux-i686.tar.bz2
	rm -rf ${APP_DIR}/xulrunner || true
	tar jxvf ${XR_DIR}/xulrunner-${XR_VERSION}.en-US.linux-i686.tar.bz2
	mv xulrunner ${APP_DIR}/

linux64: ${XR_DIR}/xulrunner-${XR_VERSION}.en-US.linux-x86_64.tar.bz2
	rm -rf ${APP_DIR}/xulrunner || true
	tar jxvf ${XR_DIR}/xulrunner-${XR_VERSION}.en-US.linux-x86_64.tar.bz2
	mv xulrunner ${APP_DIR}/

mac: ${XR_DIR}/xulrunner-${XR_VERSION}.en-US.mac.tar.bz2
	rm -rf ${APP_DIR}/xulrunner || true
	tar jxvf ${XR_DIR}/xulrunner-${XR_VERSION}.en-US.mac.tar.bz2
	mv xulrunner ${APP_DIR}/

win: ${XR_DIR}/xulrunner-${XR_VERSION}.en-US.win32.zip
	rm -rf ${APP_DIR}/xulrunner || true
	unzip ${XR_DIR}/xulrunner-${XR_VERSION}.en-US.win32.zip
	mv xulrunner ${APP_DIR}/

${XR_DIR}/xulrunner-${XR_VERSION}.en-US.linux-i686.tar.bz2:
	@(cd ${XR_DIR} && curl -O http://ftp.mozilla.org/pub/mozilla.org/xulrunner/releases/${XR_VERSION}/runtimes/xulrunner-${XR_VERSION}.en-US.linux-i686.tar.bz2)

${XR_DIR}/xulrunner-${XR_VERSION}.en-US.linux-x86_64.tar.bz2:
	@(cd ${XR_DIR} && curl -O http://ftp.mozilla.org/pub/mozilla.org/xulrunner/releases/${XR_VERSION}/runtimes/xulrunner-${XR_VERSION}.en-US.linux-x86_64.tar.bz2)

${XR_DIR}/xulrunner-${XR_VERSION}.en-US.mac.tar.bz2:
	@(cd ${XR_DIR} && curl -O http://ftp.mozilla.org/pub/mozilla.org/xulrunner/releases/${XR_VERSION}/runtimes/xulrunner-${XR_VERSION}.en-US.mac.tar.bz2)

${XR_DIR}/xulrunner-${XR_VERSION}.en-US.win32.zip:
	@(cd ${XR_DIR} && curl -O http://ftp.mozilla.org/pub/mozilla.org/xulrunner/releases/${XR_VERSION}/runtimes/xulrunner-${XR_VERSION}.en-US.win32.zip)

clean:
	@rm -rf ${CHROME_DIR}		|| true
	@rm -rf ${CONTENT}			|| true
	@rm ${APP_DIR}/${APP}		|| true
	@rm ${APP_DIR}/${APP}.exe	|| true
	@rm *.zip *.appcache		|| true

distclean: clean
	rm -rf ${APP_DIR}/xulrunner	|| true
	rm ${XR_DIR}/xulrunner-*.bz2 ${XR_DIR}/xulrunner-*.zip

all: ${APP_DIR}/xulrunner
	mkdir -p ${CONTENT}/unstable
	cp -r ../fonts ../src ${CONTENT}
	cp *.html *.css *.js ${CONTENT}/${APP}
	if [ -f ${APP_DIR}/xulrunner/*stub*.exe ]; then					\
		cp ${APP_DIR}/xulrunner/*stub* ${APP_DIR}/${APP}.exe	;	\
	else															\
		cp ${APP_DIR}/xulrunner/*stub* ${APP_DIR}/${APP}		;	\
	fi

# TODO	Implement a 'package' target that builds the correct package for the
#		platform that was just built... .deb/.rpm/.tgz/.msi/.dmg

test: all
	${APP_DIR}/${APP}

.PHONY: clean test appcache unstable.appcache

