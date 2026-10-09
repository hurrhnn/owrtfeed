# This is free software, licensed under the Apache License, Version 2.0 .

include $(TOPDIR)/rules.mk

PKG_VERSION:=1.0.3
PKG_RELEASE:=2
PKG_LICENSE:=Apache-2.0

LUCI_TITLE:=LuCI Airoha SoC Status (NPU, CPU, Frame Engine)
LUCI_DEPENDS:=+luci-base @TARGET_airoha

ifeq ($(CONFIG_TARGET_airoha_an7563),y)
  PATCH:=patch --no-backup-if-mismatch -F0
else
  PATCH_DIR:=/dev/null/an7563-patches
  ifneq ($(filter refresh update,$(MAKECMDGOALS)),)
    $(error $(notdir $(CURDIR)): patches/ is AN7563-only; refresh or update it on an airoha/an7563 config)
  endif
endif
PKG_PREPARED_DEPENDS:=CONFIG_TARGET_airoha_an7563

include $(TOPDIR)/feeds/luci/luci.mk

# call BuildPackage - OpenWrt buildroot signature
