#
# Copyright (C) 2018 Sartura Ltd.
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=mozilla-iot-gateway
PKG_NAME:=node-$(PKG_NPM_NAME)
PKG_VERSION:=0.9.3
PKG_RELEASE:=1
PKG_REV:=62fd603917d101c595de796575ffbfa3d79bd307

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/mozilla-iot/gateway.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_RELEASE)
PKG_SOURCE_VERSION:=$(PKG_REV)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_RELEASE).tar.gz
PKG_MIRROR_HASH:=c424b6f5f011c0cceb455458c855854395d47e902fd4ec2d63564c5e736d4fcd

PKG_BUILD_DEPENDS:=node/host

PKG_MAINTAINER:=Marko Ratkaj <marko.ratkaj@sartura.hr>
PKG_LICENSE:=MPL-2.0
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/node-mozilla-iot-gateway
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=WebThings Gateway by Mozilla
  URL:=https://iot.mozilla.org/gateway/
  DEPENDS:= +libpthread +node +node-npm +python +python3-light +python3-pip +openssl-util
  DEPENDS+= +MOIT_enable-plugin-support:git-http +python3-gateway-addon
  MENU:=1
endef

define Package/node-mozilla-iot-gateway/description
  Build Your Own Web of Things Gateway. The "Web of Things" (WoT) is the
  idea of taking the lessons learned from the World Wide Web and applying
  them to IoT. It's about creating a decentralized Internet of Things by
  giving Things URLs on the web to make them linkable and discoverable,
  and defining a standard data model and APIs to make them interoperable.
endef

define Package/node-mozilla-iot-gateway/config
  source "$(SOURCE)/Config.in"
endef

CPU:=$(subst powerpc,ppc,$(subst aarch64,arm64,$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))))

TARGET_CFLAGS+=$(FPIC)
define Build/Compile
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_arch=$(CONFIG_ARCH) \
	npm_config_nodedir=$(STAGING_DIR)/usr/ \
	npm_config_cache=$(TMP_DIR)/npm-cache \
	npm_config_tmp=$(TMP_DIR)/npm-tmp \
	PREFIX="$(PKG_INSTALL_DIR)/usr/" \
	$(STAGING_DIR_HOSTPKG)/bin/npm install --build-from-source --target_arch=$(CPU) -g $(DL_DIR)/$(PKG_SOURCE)
endef

define Package/node-mozilla-iot-gateway/install
	$(INSTALL_DIR) $(1)/srv/mozilla-iot/gateway/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/webthings-gateway/* $(1)/srv/mozilla-iot/gateway
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	$(STAGING_DIR_HOSTPKG)/bin/npm --prefix=$(1)/srv/mozilla-iot/gateway install \
		--build-from-source --target_arch=$(CPU) $(1)/srv/mozilla-iot/gateway
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/mozilla-iot-gateway.init $(1)/etc/init.d/mozilla-iot-gateway
endef

$(eval $(call BuildPackage,node-mozilla-iot-gateway))
