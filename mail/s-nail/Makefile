include $(TOPDIR)/rules.mk

PKG_NAME:=s-nail
PKG_VERSION:=14.8.9

PKG_RELEASE:=$(PKG_SOURCE_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=https://www.sdaoden.eu/downloads/
PKG_MD5SUM:=8ddd59edd4d5a8eadf3410875dc6501a

PKG_LICENSE:=
PKG_LICENSE_FILES:=

include $(INCLUDE_DIR)/package.mk

define Package/s-nail
  SECTION:=mail
  CATEGORY:=Mail
  DEPENDS:=+libidn +libopenssl
  TITLE:=s-nail fork of nail/heirloom-mailx
endef


#MAKE_ARGS+= CONFIG=MAXIMAL
#	CFLAGS="$(TARGET_CFLAGS)" \

MAKE_FLAGS+= \
	WANT_AUTOCC=0 \
	WANT_GSSAPI=0 \
	DESTDIR="$(PKG_INSTALL_DIR)"

define Build/Configure
	+$(MAKE_VARS) \
	$(MAKE) -C $(PKG_BUILD_DIR) $(MAKE_FLAGS) config
endef

define Build/Install
	+$(MAKE_VARS) \
	$(MAKE) -C $(PKG_BUILD_DIR) $(MAKE_INSTALL_FLAGS) install
endef

#define Build/InstallDev
#endef

define Package/s-nail/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/local/bin/s-nail $(1)/usr/bin/
endef

$(eval $(call BuildPackage,s-nail))

