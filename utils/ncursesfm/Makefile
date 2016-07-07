include $(TOPDIR)/rules.mk

PKG_NAME:=ncursesfm
PKG_VERSION:=2016-07-05
PKG_SOURCE_VERSION:=912d13272ba535e954bae780caa46d3e38cb7029

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/FedeDP/ncursesFM
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

PKG_LICENSE:=GPL-3.0
PKG_LICENSE_FILES:=COPYING
MAINTAINER:=https://github.com/plntyk/packages-misc
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/ncursesfm
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS:=+libarchive +libmagic +libncursesw +libudev
  TITLE:=ncursesFM
  URL:=https://github.com/FedeDP/ncursesFM
endef

MAKE_FLAGS+= \
	DISABLE_LIBX11=1 \
	DISABLE_LIBCONFIG=1 \
	DISABLE_LIBSYSTEMD=1 \
	DISABLE_LIBCUPS=1

define Build/Configure
endef

define Package/ncursesfm/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/ncursesFM $(1)/usr/bin
endef


$(eval $(call BuildPackage,ncursesfm))
