include $(TOPDIR)/rules.mk

PKG_NAME:=leatherman
PKG_VERSION:=0.7.5git

PKG_RELEASE:=$(PKG_SOURCE_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/puppetlabs/leatherman
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=e6a97d9e4bb9515f04b8488b00dd2a5f3cb31aed

PKG_LICENSE:=
PKG_LICENSE_FILES:=

#PKG_FIXUP:=
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk
include $(INCLUDE_DIR)/nls.mk

define Package/leatherman
  SECTION:=base
  CATEGORY:=Base
  DEPENDS:=+boost-locale +boost-log +boost-regex +libcurl
  TITLE:=leatherman
endef


CMAKE_OPTIONS+= \
	-DLEATHERMAN_SHARED:BOOL=ON


define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/leatherman
	$(CP) -r $(PKG_INSTALL_DIR)/usr/include/leatherman/* $(1)/usr/include/leatherman/
	$(INSTALL_DIR) $(1)/usr/include/boost
	$(CP) -r $(PKG_INSTALL_DIR)/usr/include/boost/* $(1)/usr/include/boost/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/lib/cmake/leatherman
	$(CP) -r $(PKG_INSTALL_DIR)/usr/lib/cmake/leatherman/* $(1)/usr/lib/cmake/leatherman/
endef


define Package/leatherman/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so $(1)/usr/lib
endef

$(eval $(call BuildPackage,leatherman))
