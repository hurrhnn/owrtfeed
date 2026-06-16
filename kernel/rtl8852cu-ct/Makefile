include $(TOPDIR)/rules.mk

PKG_NAME:=rtl8852cu-ct
PKG_RELEASE=1

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=

PKG_SOURCE_URL:=https://github.com/morrownr/rtl8852cu-20240510.git
PKG_MIRROR_HASH:=f230bdaa083eb64b4849c0e9c22d12ae68fe9bdec9980d75886237d29b35e2b9
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2024-05-10
PKG_SOURCE_VERSION:=c01bd409ddce75822cd9127ac1a97ba4fd31a9d8

PKG_MAINTAINER:=Rhnn Hur <hurrhnn@icmp.kr>
PKG_BUILD_PARALLEL:=1

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl8852cu-ct
  SUBMENU:=Wireless Drivers
  TITLE:=Driver for Realtek 8852CU Devices
  DEPENDS:=+kmod-cfg80211 +kmod-usb-core +@DRIVER_11AX_SUPPORT
  FILES:=\
	$(PKG_BUILD_DIR)/8852cu.ko
  AUTOLOAD:=$(call AutoProbe,rtl8852cu)
  PROVIDES:=kmod-rtl8852cu
endef

NOSTDINC_FLAGS := \
	$(KERNEL_NOSTDINC_FLAGS) \
    -I$(PKG_BUILD_DIR) \
    -I$(PKG_BUILD_DIR)/include \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-include backport/backport.h \
	-Wno-error=address \
	-Wno-error=missing-prototypes \
	-Wno-error=stringop-overread

NOSTDINC_FLAGS+=-DCONFIG_IOCTL_CFG80211 -DRTW_USE_CFG80211_STA_EVENT -DBUILD_OPENWRT -DCONFIG_LITTLE_ENDIAN -DCONFIG_RADIO_WORK -DCONFIG_REGD_SRC_FROM_OS -DCONFIG_FORCE_SW_CHANNEL_PLAN

define Build/Compile
    +$(KERNEL_MAKE) $(PKG_JOBS) \
        M="$(PKG_BUILD_DIR)" \
		CONFIG_RTL8852CU=m \
		CONFIG_RTL8852C=y \
        NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
        modules
endef

$(eval $(call KernelPackage,rtl8852cu-ct))
