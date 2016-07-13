include $(TOPDIR)/rules.mk

PKG_NAME:=libgcroots
PKG_VERSION:=0.2.3
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=http://sigscheme.googlecode.com/files/
PKG_MD5SUM:=3c2e0ce9f8c09ad96aa88ebf15ac028d

PKG_LICENSE:=
PKG_LICENSE_FILES:=

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libgcroots
  SECTION:=base
  CATEGORY:=Base
  DEPENDS:=
  TITLE:=gcroots
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/gcroots.pc $(1)/usr/lib/pkgconfig
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/gcroots.h $(1)/usr/include
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libgcroots.so* $(1)/usr/lib/
endef

define Package/libgcroots/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libgcroots.so* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libgcroots))

