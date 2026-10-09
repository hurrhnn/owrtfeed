# This is free software, licensed under the Apache License, Version 2.0 .

include $(TOPDIR)/rules.mk

PKG_VERSION:=1.1.9
PKG_RELEASE:=7
PKG_LICENSE:=Apache-2.0

LUCI_TITLE:=LuCI Airoha FlowSense — Offload & PPE Monitor
LUCI_DEPENDS:=+luci-base @TARGET_airoha \
	+TARGET_airoha_an7563:iw +TARGET_airoha_an7563:ip-tiny \
	+TARGET_airoha_an7563:ip-bridge +TARGET_airoha_an7563:tc-tiny

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

define Package/luci-app-airoha-flowsense/conffiles
/etc/config/npu-monitor
endef

define Package/luci-app-airoha-flowsense/postinst
#!/bin/sh
# Enforce +x on our scripts: source-tree perms have drifted to 0644 before
# (build workflow chmod missed / editor or sync resetting the mode), which
# ships a non-exec init script + rpcd backend -> "Permission denied" on the
# post-upgrade enable and an RPCError on the page. Self-heal here so the app
# works regardless of how the apk was packed. Runs offline (IPKG_INSTROOT
# set) and online.
chmod 0755 "$${IPKG_INSTROOT}/etc/init.d/npu-jitter" \
           "$${IPKG_INSTROOT}/usr/libexec/npu-jitter-daemon" \
           "$${IPKG_INSTROOT}/usr/libexec/rpcd/luci.airoha_flowsense" 2>/dev/null
[ -n "$${IPKG_INSTROOT}" ] || {
	/etc/init.d/npu-jitter enable 2>/dev/null
	/etc/init.d/npu-jitter restart 2>/dev/null
	rm -f /tmp/luci-indexcache.*
	rm -rf /tmp/luci-modulecache/
	/etc/init.d/rpcd restart 2>/dev/null
}
exit 0
endef

# call BuildPackage - OpenWrt buildroot signature
