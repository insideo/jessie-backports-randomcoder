FROM debian:jessie
MAINTAINER ccondit@randomcoder.org

RUN \
	echo "deb http://ftp.us.debian.org/debian jessie main non-free contrib" > /etc/apt/sources.list && \
	echo "deb http://security.debian.org/ jessie/updates main contrib non-free" >> /etc/apt/sources.list && \
	echo "deb-src http://ftp.us.debian.org/debian stretch main contrib non-free" >> /etc/apt/sources.list && \
	apt-get update && \
	apt-get -y dist-upgrade && \
	apt-get -y install build-essential devscripts lintian patch patchutils && \
	rm -rf /var/cache/apt && \
	mkdir -p /build /packages

RUN \
	cd /build && \
	apt-get update && \
	apt-get -y build-dep tar && \
	apt-get -y source tar && \
	rm -rf /var/cache/apt && \
	cd tar-* && \
	DEBEMAIL=ccondit@randomcoder.org DEBFULLNAME="Craig Condit" \
	dch -b -l "~local" --distribution jessie-backports-randomcoder --force-distribution --noquery \
	'Rebuild for Debian Jessie.' && \
	DEB_BUILD_OPTIONS=nocheck FORCE_UNSAFE_CONFIGURE=1 dpkg-buildpackage && \
	cd /build && \
	cp -f *~local*.deb /packages/
