#
# Copyright (C) 2006-2015 OpenWrt.org
# Copyright (C) 2011 SMBPhone Inc.
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=slang2
PKG_VERSION:=2.3.0
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/slang-$(PKG_VERSION)
PKG_SOURCE:=slang-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=http://www.jedsoft.org/releases/slang
PKG_MD5SUM:=3bcc790460d52db1316c20395b7ac2f1

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYING

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/slang2/Default
  SUBMENU:=
  TITLE:=S-Lang
  URL:=http://www.jedsoft.org/slang
  MAINTAINER:=Eugene San (eugenesan) <eugenesan@gmail.com>
endef

define Package/slang2/Default/description
  Multi-platform programmer's library providing facilities for interactive
  applications. Includes such things as display/screen management,
  keyboard input, keymaps, embeddable S-Lang interpreter, etc.
endef

define Package/libslang2
  $(call Package/sqlite3/Default)
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+terminfo
  TITLE+= (library)
endef

define Package/libslang2/description
$(call Package/slang2/Default/description)
 This package contains only the shared library libslang.so.* and copyright
 information.
endef

define Package/slsh
  $(call Package/sqlite3/Default)
  SECTION:=lang
  CATEGORY:=Languages
  DEPENDS:=+libslang2
  TITLE+= (lang)
endef

define Package/slsh/description
$(call Package/slang2/Default/description)
  This is a shell which is mostly just a wrapper around the S-Lang Interpreter,
  which is part of the S-Lang Library.
endef

TARGET_CFLAGS += $(FPIC)
TARGET_LDFLAGS += -L../src/${ARCH}elfobjs

CONFIGURE_ARGS += \
	--enable-largefile \
	--disable-warnings \
	--with-terminfo=default \
	--with-readline=slang \
	--without-x \
	--without-pcre \
	--without-onig \
	--without-png \
	--without-z \
	ac_cv_path_nc5config=$(PWD)/files/ncurses5-config

define Build/Compile
	$(call Build/Compile/Default)
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CFLAGS="$(TARGET_CFLAGS)" \
		LDFLAGS="$(TARGET_LDFLAGS)" \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		static install install-static
endef

define Package/libslang2/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libslang.so* $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/slang/ $(1)/usr/lib/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/{include,lib,lib/pkgconfig}
	$(CP) $(PKG_INSTALL_DIR)/usr/include/*.h $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libslang.{a,so*} $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/*.pc $(1)/usr/lib/pkgconfig/
endef

define Package/slsh/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/slsh $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/share
	$(CP) $(PKG_INSTALL_DIR)/usr/share/slsh/ $(1)/usr/share/
endef

$(eval $(call BuildPackage,libslang2))
$(eval $(call BuildPackage,slsh))
