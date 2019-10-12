#
# Copyright (C) 2019 CZ.NIC, z. s. p. o. (https://www.nic.cz/)
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=python-singleton-decorator
PKG_VERSION:=1.0.0
PKG_RELEASE:=1

PKG_SOURCE:=singleton-decorator-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://files.pythonhosted.org/packages/source/s/singleton-decorator/
PKG_HASH:=1a90ad8a8a738be591c9c167fdd677c5d4a43d1bc6b1c128227be1c5e03bee07

PKG_BUILD_DIR:=$(BUILD_DIR)/$(BUILD_VARIANT)-singleton-decorator-$(PKG_VERSION)

PKG_LICENSE:=GPLv3
#PKG_LICENSE_FILES:=COPYING

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/../feeds/packages/lang/python/python3-package.mk

PKG_UNPACK:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components=1 -xzf $(DL_DIR)/$(PKG_SOURCE)

define Package/python3-singleton-decorator
  SECTION:=lang
  CATEGORY:=Languages
  SUBMENU:=Python
  TITLE:=A testable singleton decorator
  URL:=https://github.com/Kemaweyan/singleton_decorator/tree/master/singleton_decorator
  DEPENDS:= +PACKAGE_python3-singleton-decorator:python3-light
  VARIANT:=python3
endef

define Package/python3-singleton-decorator/description
A testable singleton decorator
.
(Variant for Python3)
endef

$(eval $(call Py3Package,python3-singleton-decorator))
$(eval $(call BuildPackage,python3-singleton-decorator))
$(eval $(call BuildPackage,python3-singleton-decorator-src))
