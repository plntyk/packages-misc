include $(TOPDIR)/rules.mk

PKG_NAME:=kbd
PKG_VERSION:=2.0.3
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=http://ftp.kernel.org/pub/linux/utils/kbd
PKG_MD5SUM:=231b46e7142eb41ea3ae06d2ded3c208

#PKG_LICENSE:=
#PKG_LICENSE_FILES:=

PKG_INSTALL:=1
PKG_BUILD_DEPENDS:=check

include $(INCLUDE_DIR)/package.mk

define Package/kbd
  SECTION:=base
  CATEGORY:=Base
  TITLE:=kbd-utils
endef

CONFIGURE_ARGS+= --disable-vlock

define Package/kbd/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/share/keymaps
	$(INSTALL_DIR) $(1)/usr/share/keymaps/include
	$(CP) $(PKG_INSTALL_DIR)/usr/share/keymaps/i386/* $(1)/usr/share/keymaps/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/keymaps/include/* $(1)/usr/share/keymaps/include/
endef

$(eval $(call BuildPackage,kbd))

