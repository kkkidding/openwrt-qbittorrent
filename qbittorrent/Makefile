#
# Copyright (C) 2017-2020
#
# This is free software, licensed under the GNU General Public License v2.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=qbittorrent
PKG_VERSION:=4.2.5
PKG_RELEASE=1

PKG_SOURCE:=qBittorrent-release-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/qbittorrent/qBittorrent/tar.gz/release-$(PKG_VERSION)?
PKG_HASH:=1dac52d6fe4b0c44dba04fcfc41f519c57a69cb30580255edca95c87053a4324

PKG_BUILD_DIR:=$(BUILD_DIR)/qBittorrent-release-$(PKG_VERSION)

PKG_LICENSE:=GPL-2.0+
PKG_LICENSE_FILES:=COPYING

PKG_BUILD_DEPENDS:=qttools

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1
PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk

define Package/qbittorrent
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=BitTorrent
	DEPENDS:=+boost-system +libopenssl +libstdcpp +libtorrent-rasterbar +qt5-core \
		+qt5-network +qt5-xml +zlib
	TITLE:=bittorrent client programmed in C++ / Qt
	URL:=https://www.qbittorrent.org/
	PROVIDES:=qBittorrent
endef

define Package/qbittorrent/description
  qBittorrent is a bittorrent client programmed in C++ / Qt that uses
  libtorrent (sometimes called libtorrent-rasterbar) by Arvid Norberg.
  It aims to be a good alternative to all other bittorrent clients out
  there. qBittorrent is fast, stable and provides unicode support as
  well as many features.
endef

CONFIGURE_ARGS += \
	--disable-gui \
	--enable-stacktrace=no \
	--with-boost=$(STAGING_DIR)/usr

MAKE_VARS += \
	INSTALL_ROOT="$(PKG_INSTALL_DIR)"

TARGET_CXXFLAGS += -std=c++14 -ffunction-sections -fdata-sections -flto
TARGET_LDFLAGS += -Wl,--gc-sections,--as-needed

define Package/qbittorrent/conffiles
/etc/config/qbittorrent
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	$(SED) '/<context>/{:a;N;/<\/context>/!ba;/\/gui\//d}' `ls $(PKG_BUILD_DIR)/src/lang/qbittorrent_*.ts`
endef

define Package/qbittorrent/install
	$(INSTALL_DIR) $(1)/etc/init.d $(1)/etc/config
	$(INSTALL_BIN) ./files/qbittorrent.init $(1)/etc/init.d/qbittorrent
	$(INSTALL_CONF) ./files/qbittorrent.conf $(1)/etc/config/qbittorrent

	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/qbittorrent-nox $(1)/usr/bin
endef

$(eval $(call BuildPackage,qbittorrent))
