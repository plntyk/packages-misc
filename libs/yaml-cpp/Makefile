include $(TOPDIR)/rules.mk

PKG_NAME:=yaml-cpp
PKG_VERSION:=2016-06-13

PKG_RELEASE:=$(PKG_SOURCE_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/jbeder/yaml-cpp/
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=f74ae543b43c76fa98c3418df05051a64c2623b4

#PKG_LICENSE:=
#PKG_LICENSE_FILES:=

#PKG_FIXUP:=
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/yaml-cpp
  SECTION:=base
  CATEGORY:=Base
  DEPENDS:=+libstdcpp
  TITLE:=YAML parser and emitter in C++
endef

CMAKE_OPTIONS += -DBUILD_SHARED_LIBS:BOOL=ON

#define Build/Configure
#endef

#define Build/Compile
#endef

#define Build/Install
#endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libyaml-cpp* $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/lib/cmake
	$(CP) -r $(PKG_INSTALL_DIR)/usr/lib/cmake/* $(1)/usr/lib/cmake/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) -r $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/yaml-cpp.pc $(1)/usr/lib/pkgconfig
	$(INSTALL_DIR) $(1)/usr/include/yaml-cpp
	$(CP) -r $(PKG_INSTALL_DIR)/usr/include/yaml-cpp/* $(1)/usr/include/yaml-cpp/
endef

define Package/yaml-cpp/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libyaml-cpp* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,yaml-cpp))

