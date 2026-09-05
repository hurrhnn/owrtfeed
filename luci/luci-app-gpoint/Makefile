include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-gpoint
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/hurrhnn/luci-app-gpoint.git
PKG_SOURCE_DATE:=2026-09-05
PKG_SOURCE_VERSION:=d33ae1414a0f7902ab0eb59827da21d6c785d0dc
PKG_MIRROR_HASH:=3ec15ba26044a3d6cec7813416933ff5e0dda75b63e7b92b964eaf092ab16486a

include $(INCLUDE_DIR)/package.mk

define Package/luci-app-gpoint
  SECTION:=luci
  CATEGORY:=LuCI
  SUBMENU:=3. Applications
  TITLE:=GNSS Information dashboard for 3G/LTE dongle
  URL:=https://github.com/hurrhnn/luci-app-gpoint
  DEPENDS:=+lua +luci-compat +curl +lua-rs232 +luasocket +iwinfo +libiwinfo-lua +lua-bit32 +usbutils +gpsd
  PKGARCH:=all
endef

define Package/luci-app-gpoint/description
  Gpoint - GNSS (GPS/GLONASS) dashboard for LuCI. Reads NMEA data from
  3G/LTE modems or USB GNSS receivers (directly or through gpsd), shows
  the position on a map and can forward it to a tracking server.
endef

define Build/Configure
endef

define Build/Compile
endef

define Package/luci-app-gpoint/install
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci
	$(CP) $(PKG_BUILD_DIR)/luasrc/* $(1)/usr/lib/lua/luci/
	$(INSTALL_DIR) $(1)/www
	$(CP) $(PKG_BUILD_DIR)/htdocs/* $(1)/www/
	$(INSTALL_DIR) $(1)/
	$(CP) $(PKG_BUILD_DIR)/root/* $(1)/
endef

define Package/luci-app-gpoint/postinst
[ -n "$${IPKG_INSTROOT}" ] || {
	rm -f /tmp/luci-indexcache.*
	rm -rf /tmp/luci-modulecache/
	/etc/init.d/rpcd reload 2>/dev/null
	exit 0
}
endef

define Package/luci-app-gpoint/postrm
rm -f /etc/config/gpoint
endef

$(eval $(call BuildPackage,luci-app-gpoint))
