include $(TOPDIR)/rules.mk

PKG_NAME:=rtw89-ct
PKG_RELEASE=1

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=

PKG_SOURCE_URL:=https://github.com/morrownr/rtw89.git
PKG_MIRROR_HASH:=1d1347bfbcbb31dd2bc0f1bae908580b82b898cb1f712cdaf9d9a847dbc83a91
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2026-02-04
PKG_SOURCE_VERSION:=95b2936fbe92e735513daedf9abac2b1ae62189e

PKG_MAINTAINER:=Rhnn Hur <hurrhnn@icmp.kr>
PKG_BUILD_PARALLEL:=1

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtw89-ct
  SUBMENU:=Wireless Drivers
  TITLE:=Drivers for Realtek Wi-Fi 6/6E and Wi-Fi 7 adapters
  DEPENDS:=+kmod-mac80211 +kmod-cfg80211 +kmod-usb-core +@DRIVER_11AX_SUPPORT
  FILES:=\
	$(PKG_BUILD_DIR)/rtw89_core_git.ko \
	$(PKG_BUILD_DIR)/rtw89_usb_git.ko \
	$(PKG_BUILD_DIR)/rtw89_8852c_git.ko \
	$(PKG_BUILD_DIR)/rtw89_8852cu_git.ko \
	$(PKG_BUILD_DIR)/rtw89_8922a_git.ko \
	$(PKG_BUILD_DIR)/rtw89_8922au_git.ko
  AUTOLOAD:=$(call AutoProbe,rtw89_8922au_git)
  PROVIDES:=kmod-rtw89-ct
endef

define KernelPackage/rtw89-ct-firmware
  SUBMENU:=Wireless Drivers
  TITLE:=Realtek Wi-Fi 6/6E and Wi-Fi 7 specific firmware
  DEPENDS+=+kmod-rtw89-ct
endef

define KernelPackage/rtw89-ct-firmware/install
	$(INSTALL_DIR) $(1)/lib/firmware/rtw89
	cp \
		$(PKG_BUILD_DIR)/firmware/rtw8851b_fw.bin \
		$(PKG_BUILD_DIR)/firmware/rtw8852a_fw.bin \
		$(PKG_BUILD_DIR)/firmware/rtw8852c_fw-2.bin \
		$(PKG_BUILD_DIR)/firmware/rtw8852c_fw-2.bin \
		$(PKG_BUILD_DIR)/firmware/rtw8922a_fw-4.bin \
		$(1)/lib/firmware/rtw89
endef

NOSTDINC_FLAGS := \
    $(KERNEL_NOSTDINC_FLAGS) \
    -I$(STAGING_DIR)/usr/include/mac80211-backport \
    -I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
    -I$(STAGING_DIR)/usr/include/mac80211 \
    -I$(STAGING_DIR)/usr/include/mac80211/uapi \
    -include backport/backport.h \
    -Wno-error=address \
    -Wno-error=missing-prototypes \
    -Wno-error=stringop-overread

NOSTDINC_FLAGS+=-DCONFIG_IOCTL_CFG80211 -DRTW_USE_CFG80211_STA_EVENT -DBUILD_OPENWRT

define Build/Compile
	+$(KERNEL_MAKE) $(PKG_JOBS) \
		M="$(PKG_BUILD_DIR)" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		modules
endef

$(eval $(call KernelPackage,rtw89-ct))
$(eval $(call KernelPackage,rtw89-ct-firmware))
