#
# Copyright (C) 2014-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libnanomsg
PKG_VERSION:=1.1.5
PKG_RELEASE:=1
PKG_REV:=1749fd7b039165a91b8d556b4df18e3e632ad830

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/nanomsg/nanomsg.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=$(PKG_REV)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_MIRROR_HASH:=446c4b7ffc2554f7a5b79be2655c06f498d3b3b127e5c90c6cb157fd441fb315

PKG_LICENSE:=MIT/X11
PKG_LICENSE_FILE:=COPYING

PKG_FIXUP:=autoreconf -ivf

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/libnanomsg
	SECTION:=libs
	CATEGORY:=Libraries
	TITLE:=Nanomsg high-performance scalability protocol
	URL:=http://nanomsg.org/
	DEPENDS:=+libssp +librt +libpthread
endef

define Package/libnanomsg/description
	nanomsg is a socket library that provides several common communication patterns. It aims to make the networking layer fast,
	scalable, and easy to use. Implemented in C, it works on a wide range of operating systems with no further dependencies.
endef


define Package/libnanomsg/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/.libs/libnanomsg.so*  $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_BUILD_DIR)/nanocat  $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/sbin
	$(CP) $(PKG_BUILD_DIR)/tcpmuxd  $(1)/usr/sbin/
endef

$(eval $(call BuildPackage,libnanomsg))
