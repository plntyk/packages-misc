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
  DEPENDS:=+samba4-libs +samba4-server-libs
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

define Package/samba4-server-libs
  SECTION:=base
  CATEGORY:=Base
  TITLE:=samba4 SMB/CIFS client libraries
  DEPENDS:=
  URL:=http://www.samba.org
endef

define Package/samba4-testparm
  SECTION:=base
  CATEGORY:=Base
  TITLE:=samba4 testparm
  URL:=http://www.samba.org
endef

define Package/samba4-multicall
  SECTION:=base
  CATEGORY:=Base
  TITLE:=samba4 multicall binary
  DEPENDS:=+libcap +libpopt +zlib
  URL:=http://www.samba.org
endef

SAMBA4_IDMAP_MODULES_SHARED := idmap_ad,idmap_rid,idmap_adex,idmap_hash,idmap_tdb2,
SAMBA4_IDMAP_MODULES_STATIC := 
SAMBA4_PDB_MODULES_SHARED := pdb_wbc_sam,pdb_samba4,
SAMBA4_PDB_MODULES_STATIC := pdb_smbpasswd,pdb_tdbsam,
SAMBA4_AUTH_MODULES_SHARED := auth_wbc,auth_script,auth_samba4
SAMBA4_AUTH_MODULES_STATIC := auth_sam,auth_unix,
SAMBA4_VFS_MODULES_STATIC := vfs_default,vfs_aio_pthread

SAMBA4_MODULES_SHARED = "${SAMBA4_IDMAP_MODULES_SHARED}${SAMBA4_PDB_MODULES_SHARED}${SAMBA4_AUTH_MODULES_SHARED}${SAMBA4_VFS_MODULES_SHARED}"
SAMBA4_MODULES_STATIC = "${SAMBA4_IDMAP_MODULES_STATIC}${SAMBA4_PDB_MODULES_STATIC}${SAMBA4_AUTH_MODULES_STATIC}${SAMBA4_VFS_MODULES_STATIC}"


EXTRA_CFLAGS += \
	-ffunction-sections \
	-fdata-sections \
	-Wno-deprecated-declarations

EXTRA_LDFLAGS += -Wl,--gc-sections

CONF_ARGS = \
	--cross-compile \
	--cross-answers=$(PKG_BUILD_DIR)/target.txt \

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
	--without-dnsupdate \
	--without-dmapi \
	--without-fam \
	--without-gettext \
	--without-iconv \
	--without-ldap \
	--without-libarchive \
	--without-lttng \
	--without-pam \
	--without-pie \
	--without-quotas \
	--without-regedit \
	--without-systemd \
	--without-utmp \
	--without-winbind \
	--hostcc=gcc \
	--nopyc \
	--nopyo \
	--with-configdir=/etc/samba \
	--with-lockdir=/var/samba/locks \
	--with-piddir=/var/run \
	--with-privatedir=/var/samba/private \
	--with-sockets-dir=/var/samba/sockets \
	--enable-auto-reconfigure \
	--nonshared-binary=$(PKG_NAME)_multicall \
	--bundled-libraries='!asn1_compile,!compile_et'

#	--with-static-modules=${SAMBA4_MODULES_STATIC} \
#	--with-shared-modules=${SAMBA4_MODULES_SHARED} \
#	--bundled-libraries='!asn1_compile,!compile_et,!zlib,popt,talloc,pytalloc,pytalloc-util'


define Build/Prepare
	$(call Build/Prepare/Default)
	$(SED) 's#__OPENWRT_SAMBA_MULTICALL__#$(PKG_NAME)_multicall#g' $(PKG_BUILD_DIR)/source3/wscript_build
	$(CP) ./target.txt $(PKG_BUILD_DIR)/
	echo "Checking uname machine type: \"$(CONFIG_ARCH)\"" >> $(PKG_BUILD_DIR)/target.txt
endef


PYTHON_ENV:= \
	PYTHON_CONFIG="$(STAGING_DIR)/usr/bin/python2-config" \
	python_LDFLAGS="" \
	python_LIBDIR=""


define Build/Configure
	(cd $(PKG_BUILD_DIR); \
	$(SAMBA4_CONF_VARS) \
	$(PYTHON_ENV) \
	$(TARGET_CONFIGURE_OPTS) \
	./buildtools/bin/waf configure -j$(shell nproc) \
	$(CONF_ARGS) \
	$(SAMBA4_CONF_ARGS) \
	)
endef


define Build/Compile
	(cd $(PKG_BUILD_DIR); \
		./buildtools/bin/waf build \
			--targets=$(PKG_NAME)_multicall \
	)
endef

define Package/samba4-server/conffiles
/etc/config/samba4
/etc/samba/smb.conf.template
/etc/samba/smbpasswd
endef


define Package/samba4-libs/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libCHARSET3-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libaddns-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libcli-smb-common-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libdcerpc-samba-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libgenrand-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/liblibsmb-samba4.so $(1)/usr/lib
#client
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libpopt-samba3-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsamba-debug-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libsamba-util.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libserver-role-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libsmbconf.so* $(1)/usr/lib


# pam depends?
#	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libpopt-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsamba-security-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsamba-sockets-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsamba3-util-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libsmbregistry-samba4.so $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libtalloc.so.2* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/private/libtevent.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libndr-standard.so* $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/local/samba/lib/libsamba-errors.so* $(1)/usr/lib
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
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/samba4.init $(1)/etc/init.d/samba4
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/samba4.config $(1)/etc/config/samba4
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/local/samba/sbin/nmbd $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/local/samba/sbin/smbd $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc/samba
	$(INSTALL_DATA) ./files/smb.conf.template $(1)/etc/samba
endef

define Package/samba4-server-libs/install
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

define Package/samba4-client/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/local/samba/bin/smbclient $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/local/samba/bin/nmblookup $(1)/usr/sbin
endef

define Package/samba4-testparm/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/local/samba/bin/testparm $(1)/usr/bin
endef

define Package/samba4-multicall/install
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/$(PKG_NAME).config $(1)/etc/config/samba4
	$(INSTALL_DIR) $(1)/etc/samba
	$(INSTALL_DATA) ./files/smb.conf.template $(1)/etc/samba
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/$(PKG_NAME).init $(1)/etc/init.d/samba4
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/default/source3/samba4_multicall $(1)/usr/sbin
	$(LN) samba4_multicall $(1)/usr/sbin/smbd
	$(LN) samba4_multicall $(1)/usr/sbin/nmbd
	$(LN) samba4_multicall $(1)/usr/sbin/smbpasswd
	$(LN) samba4_multicall $(1)/usr/sbin/pdbedit
	$(INSTALL_DIR) $(1)/usr/bin
	$(LN) ../sbin/samba4_multicall $(1)/usr/bin/smbcontrol
	$(LN) ../sbin/samba4_multicall $(1)/usr/bin/smbtree
	$(LN) ../sbin/samba4_multicall $(1)/usr/bin/smbstatus
	$(LN) ../sbin/samba4_multicall $(1)/usr/bin/smbcquotas
	$(LN) ../sbin/samba4_multicall $(1)/usr/bin/smbcacls
	$(LN) ../sbin/samba4_multicall $(1)/usr/bin/nmblookup
	$(LN) ../sbin/samba4_multicall $(1)/usr/bin/testparm
endef

$(eval $(call BuildPackage,samba4-libs))
$(eval $(call BuildPackage,samba4-server))
$(eval $(call BuildPackage,samba4-server-libs))
$(eval $(call BuildPackage,samba4-client))
$(eval $(call BuildPackage,samba4-testparm))
$(eval $(call BuildPackage,samba4-multicall))
