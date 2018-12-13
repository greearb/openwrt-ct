include $(TOPDIR)/rules.mk

PKG_NAME:=rtl8812au-ct
PKG_RELEASE=1

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=

PKG_SOURCE_URL:=https://github.com/greearb/rtl8812AU_8821AU_linux.git
PKG_MIRROR_HASH:=7f4b79a828c53b6d8415a1851c1548574f18849cdef625ae1a1bcb94106b5b97
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2018-11-13
PKG_SOURCE_VERSION:=8593dab5a414cc57301bd9ea4a04a3747a8f8dce

PKG_MAINTAINER:=Ben Greear <greearb@candelatech.com>
PKG_BUILD_PARALLEL:=1
#PKG_EXTMOD_SUBDIRS:=rtl8812au-ct

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl8812au-ct
  SUBMENU:=Wireless Drivers
  TITLE:=Driver for Realtek 8812 AU devices comfast 912-ac, etc
  DEPENDS:=+kmod-mac80211 +@DRIVER_11N_SUPPORT +@DRIVER_11AC_SUPPORT
  FILES:=\
	$(PKG_BUILD_DIR)/rtl8812au.ko
  AUTOLOAD:=$(call AutoProbe,rtl8812au)
  PROVIDES:=kmod-rtl8812au
endef

NOSTDINC_FLAGS = \
	-I$(PKG_BUILD_DIR) \
	-I$(PKG_BUILD_DIR)/include \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-include backport/backport.h

NOSTDINC_FLAGS+=-DCONFIG_IOCTL_CFG80211 -DRTW_USE_CFG80211_STA_EVENT -DBUILD_OPENWRT

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		$(KERNEL_MAKE_FLAGS) \
		SUBDIRS="$(PKG_BUILD_DIR)" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		modules
endef

$(eval $(call KernelPackage,rtl8812au-ct))
