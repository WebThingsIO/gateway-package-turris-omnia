#
# Copyright (C) 2019 CZ.NIC, z. s. p. o. (https://www.nic.cz/)
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=python-nnpy
PKG_VERSION:=1.4.2
PKG_RELEASE:=1

PKG_SOURCE:=nnpy-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://files.pythonhosted.org/packages/source/n/nnpy/
PKG_HASH:=52c1612f116294a7c87a27e49e027f3b55baefc9d279fbbf70e84d315d430ff9

PKG_BUILD_DIR:=$(BUILD_DIR)/$(BUILD_VARIANT)-nnpy-$(PKG_VERSION)

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE.mit
PKG_MAINTAINER:=Josef Schlehofer <josef.schlehofer@nic.cz>

PKG_BUILD_DEPENDS:=libffi/host
HOST_PYTHON3_PACKAGE_BUILD_DEPENDS:="cffi>=1.1"

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/../feeds/packages/lang/python/python3-package.mk

PKG_UNPACK:=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components=1 -xzf $(DL_DIR)/$(PKG_SOURCE)

define Package/python3-nnpy
  SECTION:=lang
  CATEGORY:=Languages
  SUBMENU:=Python
  TITLE:=cffi-based Python bindings for nanomsg
  URL:=https://github.com/nanomsg/nnpy
  DEPENDS:= \
		+PACKAGE_python3-nnpy:python3-light \
		+PACKAGE_python3-nnpy:python3-cffi \
        +PACKAGE_python3-nnpy:libnanomsg
  VARIANT:=python3
endef

define Package/python3-nnpy/description
cffi-based Python bindings for nanomsg
.
(Variant for Python3)
endef

$(eval $(call Py3Package,python3-nnpy))
$(eval $(call BuildPackage,python3-nnpy))
$(eval $(call BuildPackage,python3-nnpy-src))
