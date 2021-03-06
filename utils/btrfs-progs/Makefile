#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=btrfs-progs
PKG_VERSION:=5.1.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-v$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=@KERNEL/linux/kernel/people/kdave/btrfs-progs
PKG_HASH:=9cb91b7de9e10aa6bbf2b003f60bb3f5e5b1984a8008fad7c4b2d3978f5ebe1b
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-v$(PKG_VERSION)

PKG_MAINTAINER:=Karel Kočí <karel.koci@nic.cz>
PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYING

PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1
PKG_BUILD_DEPENDS:=acl

include $(INCLUDE_DIR)/package.mk

define Package/btrfs-progs
  SECTION:=utils
  CATEGORY:=Utilities
  SUBMENU:=Filesystem
  TITLE:=Btrfs filesystems utilities
  URL:=https://btrfs.wiki.kernel.org/
  DEPENDS:=+libattr +libuuid +zlib +libblkid +liblzo +libpthread +BTRFS_PROGS_ZSTD:libzstd
endef

define Package/btrfs-progs/description
 Btrfs is a new copy on write filesystem for Linux aimed at implementing
 advanced features while focusing on fault tolerance, repair and easy
 administration. Initially developed by Oracle, Btrfs is licensed under the
 GPL and open for contribution from anyone.
endef

define Package/btrfs-progs/config
	source "$(SOURCE)/Config.in"
endef

progs = btrfs btrfs-find-root btrfs-image btrfs-map-logical \
	btrfs-select-super btrfstune mkfs.btrfs

TARGET_CFLAGS += -ffunction-sections -fdata-sections
TARGET_LDFLAGS += -Wl,--gc-sections -Wl,--as-needed

EXTRA_CFLAGS=$(TARGET_CPPFLAGS)

CONFIGURE_ARGS += \
	--disable-backtrace \
	--disable-convert \
	--disable-documentation \
	--disable-python
ifneq ($(CONFIG_BTRFS_PROGS_ZSTD),y)
CONFIGURE_ARGS += --disable-zstd
endif

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libbtrfs* $(1)/usr/lib/
endef

define Package/btrfs-progs/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libbtrfs.so* $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libbtrfsutil.so* $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(addprefix $(PKG_INSTALL_DIR)/usr/bin/, $(progs)) $(1)/usr/bin/
	$(LN) btrfs $(1)/usr/bin/btrfsck
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/btrfs-scan.init $(1)/etc/init.d/btrfs-scan
endef

$(eval $(call BuildPackage,btrfs-progs))
