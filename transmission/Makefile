#
# Copyright (C) 2009-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=transmission
PKG_VERSION:=2.94
PKG_RELEASE:=9

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=@GITHUB/transmission/transmission-releases/master
PKG_HASH:=35442cc849f91f8df982c3d0d479d650c6ca19310a994eccdaa79a4af3916b7d

PKG_MAINTAINER:=Rosen Penev <rosenp@gmail.com>
PKG_LICENSE:=GPL-2.0-or-later
PKG_LICENSE_FILES:=COPYING
PKG_CPE_ID:=cpe:/a:transmissionbt:transmission

PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/transmission/template
  SUBMENU:=BitTorrent
  SECTION:=net
  CATEGORY:=Network
  TITLE:=BitTorrent client
  URL:=https://www.transmissionbt.com
  DEPENDS:=+libcurl +libevent2 +libminiupnpc +libnatpmp +libpthread +librt +zlib
endef

define Package/transmission-daemon/Default
  $(call Package/transmission/template)
  USERID:=transmission=224:transmission=224
endef

define Package/transmission-daemon
  $(call Package/transmission-daemon/Default)
  TITLE+= (with mbed TLS)
  DEPENDS+=+libmbedtls
  VARIANT:=mbedtls
endef

define Package/transmission-cli
  $(call Package/transmission/template)
  TITLE+= (with mbed TLS)
  DEPENDS+=+libmbedtls
  VARIANT:=mbedtls
endef

define Package/transmission-remote
  $(call Package/transmission/template)
  TITLE+= (with mbed TLS)
  DEPENDS+=+libmbedtls
  VARIANT:=mbedtls
endef

define Package/transmission-web
  $(call Package/transmission/template)
  TITLE+= (webinterface)
  DEPENDS:=+transmission-daemon
  PKGARCH:=all
endef


define Package/transmission-daemon/Default/description
 Transmission is a simple BitTorrent client.
 It features a very simple, intuitive interface
 on top on an efficient, cross-platform back-end.
 This package contains the daemon itself.
endef
Package/transmission-daemon/description = $(Package/transmission-daemon/Default/description)

define Package/transmission-cli/Default/description
 CLI utilities for transmission.
endef
Package/transmission-cli/description = $(Package/transmission-cli/Default/description)

define Package/transmission-remote/Default/description
 CLI remote interface for transmission.
endef
Package/transmission-remote/description = $(Package/transmission-remote/Default/description)

define Package/transmission-web/description
 Webinterface resources for transmission.
endef

define Package/transmission-daemon/conffiles
/etc/config/transmission
endef

TARGET_CFLAGS += -ffunction-sections -fdata-sections -flto
TARGET_LDFLAGS += -Wl,--gc-sections -Wl,--as-needed

CONFIGURE_ARGS += \
	--enable-cli \
	--enable-daemon \
	--enable-external-natpmp \
	--enable-largefile \
	--enable-lightweight \
	--without-gtk \
	--without-kqueue \
	--without-systemd-daemon

ifeq ($(BUILD_VARIANT),mbedtls)
  CONFIGURE_ARGS += --with-crypto=polarssl
  CONFIGURE_VARS += \
	MBEDTLS_CFLAGS="-I$(STAGING_DIR)/usr/include/mbedtls" \
	MBEDTLS_LIBS="-lmbedtls -lmbedcrypto"
endif

define Package/transmission-daemon/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/transmission-daemon $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) files/transmission.init $(1)/etc/init.d/transmission
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) files/transmission.config $(1)/etc/config/transmission
	$(INSTALL_DIR) $(1)/etc/sysctl.d/
	$(INSTALL_CONF) files/transmission.sysctl $(1)/etc/sysctl.d/20-transmission.conf
endef

define Package/transmission-cli/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/transmission-cli \
			$(PKG_INSTALL_DIR)/usr/bin/transmission-create \
			$(PKG_INSTALL_DIR)/usr/bin/transmission-edit \
			$(PKG_INSTALL_DIR)/usr/bin/transmission-show \
			$(1)/usr/bin/
endef

define Package/transmission-remote/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/transmission-remote $(1)/usr/bin/
endef

define Package/transmission-web/install
	$(INSTALL_DIR) $(1)/usr/share/transmission
	$(CP) $(PKG_INSTALL_DIR)/usr/share/transmission/web $(1)/usr/share/transmission/
endef

$(eval $(call BuildPackage,transmission-daemon))
$(eval $(call BuildPackage,transmission-cli))
$(eval $(call BuildPackage,transmission-remote))
$(eval $(call BuildPackage,transmission-web))
