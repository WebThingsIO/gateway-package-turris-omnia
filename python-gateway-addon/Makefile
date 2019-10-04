#
# Copyright (C) 2019 CZ.NIC, z. s. p. o. (https://www.nic.cz/)
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=python-gateway-addon
PKG_VERSION:=0.10.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/mozilla-iot/gateway-addon-python.git
PKG_SOURCE_DATE:=2019-10-01
PKG_SOURCE_VERSION:=d43c8ceb5fa6adada8a60a8d0d6393024956640a

PKG_LICENSE:=MPL-2.0
PKG_LICENSE_FILES:=LICENSE.txt

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/../feeds/packages/lang/python/python3-package.mk

define Package/python3-gateway-addon
  SECTION:=lang
  CATEGORY:=Languages
  SUBMENU:=Python
  TITLE:=Python bindings for developing add-ons for Mozilla WebThings Gateway
  URL:=https://github.com/mozilla-iot/gateway-addon-python
  DEPENDS:= \
		+PACKAGE_python3-gateway-addon:python3-light \
		+PACKAGE_python3-gateway-addon:python3-jsonschema \
		+PACKAGE_python3-gateway-addon:python3-nnpy
  VARIANT:=python3
endef

define Package/python3-gateway-addon/description
Python bindings for developing add-ons for Mozilla WebThings Gateway
.
(Variant for Python3)
endef

$(eval $(call Py3Package,python3-gateway-addon))
$(eval $(call BuildPackage,python3-gateway-addon))
$(eval $(call BuildPackage,python3-gateway-addon-src))
