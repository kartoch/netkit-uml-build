include ../Makefile.am

BUILD_DIR=build
CACHE_DIR=cache
FINAL_ARCHIVE=../${FS_ARCHIVE_FILE}
DEBOOTSTRAP_ARCHIVE=${CACHE_DIR}/debootstrap-cache.tgz
NETKIT_DIR=${BUILD_DIR}/netkit-ng
FS_DIR=${NETKIT_DIR}/fs
MODULES_PATH=../kernel/build/netkit-ng/kernel/modules/lib/uml/modules/
NAMESERVER=$(shell grep nameserver /etc/resolv.conf | cut -d\  -f2 | tail -n 1)

default: ${FINAL_ARCHIVE}

.PHONY: clean
clean:
	rm -rf ${BUILD_DIR}

.PHONY: mrproper
mrproper: clean
	rm -rf ${CACHE_DIR}
	rm -rf ${FINAL_ARCHIVE}

.PHONY: init
init: clean
	mkdir -p ${FS_DIR}

${FINAL_ARCHIVE}: init ${DEBOOTSTRAP_ARCHIVE} ${MODULES_PATH}
	cp -r ${MODULES_PATH} ${BUILD_DIR}
	echo "Netkit-NG filesystem version ${NETKIT_FS_RELEASE}" > ${FS_DIR}/netkit-filesystem-version
	sed -i "s/^nameserver=\([0-9]\{1,3\}\.\)\{3\}[0-9]\{1,3\}/nameserver=${NAMESERVER}/" rootstrap.conf
	rootstrap  ${FS_DIR}/netkit-fs-${SUBARCH}-${NETKIT_FS_RELEASE}
	cd ${FS_DIR} ; ln -s netkit-fs-${SUBARCH}-${NETKIT_FS_RELEASE} netkit-fs
	mv ${FS_DIR}/netkit-fs-${SUBARCH}-${NETKIT_FS_RELEASE} ${FS_DIR}/netkit-fs-${SUBARCH}-${NETKIT_FS_RELEASE}.old
	cp --sparse=always ${FS_DIR}/netkit-fs-${SUBARCH}-${NETKIT_FS_RELEASE}.old ${FS_DIR}/netkit-fs-${SUBARCH}-${NETKIT_FS_RELEASE}
	rm -f ${FS_DIR}/netkit-fs-${SUBARCH}-${NETKIT_FS_RELEASE}.old
	cd ${BUILD_DIR}/ ; tar -cvSjf ../${FINAL_ARCHIVE} netkit-ng/

${DEBOOTSTRAP_ARCHIVE}:
	apt-get install debootstrap rootstrap
	mkdir -p ${BUILD_DIR}/debootstrap/
	mkdir -p ${CACHE_DIR}
	debootstrap --make-tarball=${DEBOOTSTRAP_ARCHIVE} wheezy ${BUILD_DIR}/debootstrap/ http://ftp.fr.debian.org/debian

