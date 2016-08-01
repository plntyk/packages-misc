#

include $(TOPDIR)/rules.mk

PKG_NAME:=samba4
PKG_VERSION:=4.4.5
PKG_RELEASE:=1

PKG_SOURCE:=samba-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://download.samba.org/pub/samba/
PKG_MD5SUM:=6950c5e9f7bdeb8a610c2ca957a15be4

PKG_LICENSE:=GPL3
PKG_LICENSE_FILES:=

PKG_BUILD_DIR:=$(BUILD_DIR)/samba-$(PKG_VERSION)
PKG_INSTALL:=1
PKG_BUILD_DEPENDS:=python/host python heimdal/host e2fsprogs

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk

define Package/samba4-server
  SECTION:=base
  CATEGORY:=Base
  TITLE:=samba4 SMB/CIFS server
  DEPENDS:=+samba4-libs
  URL:=http://www.samba.org
endef

define Package/samba4-client
  SECTION:=base
  CATEGORY:=Base
  TITLE:=samba4 SMB/CIFS client
  DEPENDS:=+samba4-libs
  URL:=http://www.samba.org
endef

define Package/samba4-libs
  SECTION:=base
  CATEGORY:=Base
  TITLE:=samba4 SMB/CIFS client
  DEPENDS:=+libcap +libpopt +zlib
  URL:=http://www.samba.org
endef

CONF_ARGS = \
	--cross-compile \
	--cross-answers=$(PKG_BUILD_DIR)/target.txt \


SAMBA4_CONF_VARS:= HAVE_SECURITY_PAM_APPL_H=0


SAMBA4_CONF_ARGS:= \
	--disable-avahi \
	--disable-cups \
	--disable-glusterfs \
	--disable-gnutls \
	--disable-iprint \
	--disable-rpath \
	--disable-rpath-install \
	--without-acl-support \
	--without-ads \
	--without-ad-dc \
	--without-automount \
	--without-cluster-support \
	--without-dmapi \
	--without-fam \
	--without-gettext \
	--without-iconv \
	--without-ldap \
	--without-libarchive \
	--without-lttng \
	--without-pam \
	--without-regedit \
	--hostcc=gcc \
	--nopyc \
	--nopyo \
	--with-configdir=/etc/samba \
	--with-lockdir=/var/samba \
	--with-sockets-dir=/var/samba \
	--with-piddir=/var/run \
	--with-privatedir=/var/samba/private \
	--bundled-libraries='!asn1_compile,!compile_et'


define Build/Prepare
	$(call Build/Prepare/Default)
	$(CP) $(TOPDIR)/package/network/services/samba4/target.txt $(PKG_BUILD_DIR)/
	echo "Checking uname machine type: \"$(CONFIG_ARCH)\"" >> $(PKG_BUILD_DIR)/target.txt
endef

#./build_dir/target-mips_34kc_musl-1.1.14/Python-2.7.11/ipkg-install/usr/bin/python-config
#	PYTHON_CONFIG="$(TOPDIR)/build_dir/target-mips_34kc_musl-1.1.14/Python-2.7.11/ipkg-install/usr/bin/python-config"
#	python_prefix="$(STAGING_DIR)/usr"  \
# CPP=$(TARGET_CROSS)cpp \
#	python_CFLAGS="-I$(STAGING_DIR)/usr/include/python2.7" \
#	python_LDFLAGS="-L$(STAGING_DIR)/usr/lib -lpython2.7 -lpthread -ldl -lutil -lm -Xlinker -export-dynamic" \
#	INCLUDEPY="$(STAGING_DIR)/usr/include/python2.7" \

#	PATH="$(STAGING_DIR)/usr" \


PYTHON_ENV:= \
	PYTHON="$(STAGING_DIR)/host/bin/python2" \
	PYTHON_CONFIG="$(STAGING_DIR)/host/bin/python-config" \
	python_LDFLAGS="" \
	python_LIBDIR="" \


define Build/Configure
	(cd $(PKG_BUILD_DIR); \
	$(SAMBA4_CONF_VARS) \
	$(PYTHON_ENV) \
	$(TARGET_CONFIGURE_OPTS) \
	./configure \
	$(CONF_ARGS) \
	$(SAMBA4_CONF_ARGS) \
	)
endef

#define Build/Compile
#	(cd $(PKG_BUILD_DIR); \
#	PYTHON=python2 ./configure \
#	)
#endef

define Package/samba36-server/conffiles
/etc/samba/smb.conf
endef

define Package/samba4-libs/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libCHARSET3-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libaddns-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libcli-smb-common-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libdcerpc-samba-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libgenrand-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/liblibsmb-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libpopt-samba3-samba4.so $(1)/usr/lib
# pam depends?
#	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libpopt-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsamba-debug-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsamba-security-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsamba-sockets-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsamba3-util-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsmbregistry-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libtalloc.so.2* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libtevent.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libndr-standard.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libsamba-errors.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libsamba-util.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libsmbconf.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libtevent-util.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libutil-cmdline-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libasn1util-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libcli-cldap-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libcliauth-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libcom_err-samba4.so.0* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libdbwrap-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libdcerpc-binding.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libgensec-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libgse-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libgssapi-samba4.so.2* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libinterfaces-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libiov-buf-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libkrb5samba-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libmessages-dgm-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libmessages-util-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libndr-nbt.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libndr-samba-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libndr.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libreplace-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsamba-cluster-support-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libsamba-credentials.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libsamba-hostconfig.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsecrets3-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libserver-id-db-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libserver-role-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsmb-transport-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsmbd-shim-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsocket-blocking-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsys-rw-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libtalloc-report-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libtdb-wrap-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libtdb.so.1* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libtevent-unix-util.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libtime-basic-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libutil-reg-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libutil-setid-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libutil-tdb-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libasn1-samba4.so.8* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libauthkrb5-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libcli-ldap-common-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libcli-nbt-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libhcrypto-samba4.so.5* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libheimbase-samba4.so.1* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libkrb5-samba4.so.26* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libldb.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libldbsamba-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libmsghdr-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libroken-samba4.so.19* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsamba-modules-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsamdb-common-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libsamdb.so.0* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libwbclient.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libauth-sam-reply-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libflag-mapping-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libhx509-samba4.so.5* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libndr-krb5pac.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libwinbind-client-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libwind-samba4.so.0* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libmsrpc3-samba4.so $(1)/usr/lib
endef

define Package/samba4-server/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/local/samba/sbin/nmbd $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/local/samba/sbin/smbd $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc/samba
	$(INSTALL_DATA) ./files/smb.conf $(1)/etc/samba
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libauth-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsmbd-base-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libads-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libcli-spoolss-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/liblibcli-lsa3-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/liblibcli-netlogon3-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libndr-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libnpa-tstream-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libprinting-migrate-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsmbd-conn-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libsamba-passdb.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libnetapi.so* $(1)/usr/lib
endef
#	$(INSTALL_DIR) $(1)/var/samba/private
#	$(INSTALL_DIR) $(1)/var/sock

define Package/samba4-client/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/local/samba/bin/smbclient $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/local/samba/bin/nmblookup $(1)/usr/sbin
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,samba4-libs))
$(eval $(call BuildPackage,samba4-server))
$(eval $(call BuildPackage,samba4-client))

