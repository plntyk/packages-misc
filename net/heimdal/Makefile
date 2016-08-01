include $(TOPDIR)/rules.mk

PKG_NAME:=heimdal
PKG_VERSION:=2016-06-23

PKG_RELEASE:=$(PKG_SOURCE_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/heimdal/heimdal
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=f32fd2d56ddf6f848b5fe2139376d852a3af70bc

PKG_LICENSE:=
PKG_LICENSE_FILES:=

PKG_BUILD_DEPENDS:=libncurses e2fsprogs heimdal/host
#requires perl-json for der-protos.h

PKG_BUILD_PARALLEL:=1
PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk

define Package/heimdal
  SECTION:=base
  CATEGORY:=Base
  DEPENDS:=+libncurses
  TITLE:=heimdal krb5
endef

#CONFIGURE_VARS+= ASN1_COMPILE="$(STAGING_DIR)/host/usr/bin/asn1_compile"

#CONFIGURE_ARGS+= \
#	--disable-shared \
#	--enable-static \
#	--without-openldap \
#	--without-capng \
#	--without-sqlite3 \
#	--without-libintl \
#	--without-openssl \
#	--without-berkeley-db \
#	--without-readline \
#	--without-libedit \
#	--without-hesiod \
#	--without-x \
#	--disable-heimdal-documentation

#	--without-berkeley-db \


HOST_CONFIGURE_ARGS+= \
	--disable-shared \
	--enable-static \
	--without-openldap \
	--without-capng \
	--without-sqlite3 \
	--without-readline \
	--without-libedit \
	--without-libintl \
	--without-openssl \
	--without-x \
	--disable-heimdal-documentation \
	--without-hesiod

define Host/Install
	$(MAKE) -C $(HOST_BUILD_DIR) PREFIX=$(STAGING_DIR)/host/ install
	$(INSTALL_DIR) $(STAGING_DIR)/host/bin/
	ln -sf $(STAGING_DIR)/host/libexec/heimdal/asn1_compile $(STAGING_DIR)/host/bin/asn1_compile
	ln -sf $(STAGING_DIR)/host/libexec/heimdal/slc $(STAGING_DIR)/host/bin/slc
endef

define Build/Configure
endef
define Build/Compile
endef
define Build/Install
endef

define Build/InstallDev
	$(INSTALL_DIR) $(STAGING_DIR)/host/bin
	$(INSTALL_DIR) $(1)/usr/lib/
endef

define Package/heimdal/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/lib
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,heimdal))
