#
# Copyright (C) 2012-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.

include $(TOPDIR)/rules.mk

PKG_NAME:=hnetd
PKG_SOURCE_DATE:=2018-12-21
PKG_SOURCE_VERSION:=c43766610ed30194b048bc070a3c433aec731c40
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/sbyx/hnetd
PKG_MIRROR_HASH:=3b3bfff16b99dfd23b74932ac6a74af18cb9f8fd029980f1bd3788ddfb0414bb

PKG_MAINTAINER:=Steven Barth <cyrus@openwrt.org>
PKG_LICENSE:=GPL-2.0-only
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

# Spammy debug builds for now
CMAKE_OPTIONS += -DL_LEVEL=7

# OpenWRT target
CMAKE_OPTIONS += -DBACKEND=openwrt

ifeq ($(BUILD_VARIANT),openssl)
CMAKE_OPTIONS += -DDTLS_OPENSSL=1
endif

define Package/hnetd/Default
  SECTION:=net
  CATEGORY:=Network
  TITLE:=HNCP Homenet daemon - $(2)
  URL:=https://github.com/sbyx/hnetd
  DEPENDS:=+odhcpd +odhcp6c +netifd $(3)
  DEPENDS+=@IPV6
  VARIANT:=$1
endef

Package/hnetd-nossl=$(call Package/hnetd/Default,nossl,no authentication)
Package/hnetd-openssl=$(call Package/hnetd/Default,openssl,authentication via OpenSSL,+libopenssl)

define Package/hnet-full
  SECTION:=net
  CATEGORY:=Network
  TITLE:=HNCP Homenet metapackage
  URL:=https://github.com/sbyx/hnetd
  DEPENDS:=+hnetd-nossl +luci-app-hnet +ip
  # Routing
  DEPENDS+=+babeld
  # Service discovery
  DEPENDS+=+ohybridproxy +zonestitcher
  # Distributed PCP support
  DEPENDS+=+miniupnpd +minimalist-pcproxy
endef

define Package/hnet-full-secure
  SECTION:=net
  CATEGORY:=Network
  TITLE:=HNCP Homenet metapackage (w/ SSL)
  URL:=https://github.com/sbyx/hnetd
  DEPENDS:=+hnetd-openssl +luci-app-hnet +ip
  # Routing
  DEPENDS+=+babeld
  # Service discovery
  DEPENDS+=+ohybridproxy +zonestitcher
  # Distributed PCP support
  DEPENDS+=+miniupnpd +minimalist-pcproxy
endef

define Package/hnet-full-l2tp
  SECTION:=net
  CATEGORY:=Network
  TITLE:=HNCP Homenet metapackage (w/ L2TP)
  URL:=https://github.com/sbyx/hnetd
  DEPENDS:=+hnetd-nossl +luci-app-hnet +ip-full +kmod-l2tp-eth
  # Routing
  DEPENDS+=+babeld
  # Service discovery
  DEPENDS+=+ohybridproxy +zonestitcher
  # Distributed PCP support
  DEPENDS+=+miniupnpd +minimalist-pcproxy
endef

define Package/luci-app-hnet
  SECTION:=luci
  CATEGORY:=LuCI
  SUBMENU:=3. Applications
  TITLE:=HNCP Homenet configuration and visualization
#  DEPENDS:=+hnetd
# TBD - how to express dependency on 'some' hnetd?
endef

define Package/hnetd-$(BUILD_VARIANT)/description
This package provides a daemon which implementats distributed prefix assignment
and service discovery for a home network consisting of multiple routers
connected to multiple service providers. It provides a netifd protocol "hnet"
for use in /etc/config/network.
endef

define Package/hnetd-$(BUILD_VARIANT)/install
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/hnetd $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/lib/netifd/proto
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/openwrt/hnet.sh $(1)/lib/netifd/proto
	ln -s hnetd $(1)/usr/sbin/hnet-ifresolve
	ln -s hnetd $(1)/usr/sbin/hnet-trust
	ln -s hnetd $(1)/usr/sbin/hnet-dump
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/hnetd.init $(1)/etc/init.d/hnetd
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/hnet.config $(1)/etc/config/hnet
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/generic/ohp.script $(1)/usr/sbin/hnetd-ohp-script
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/generic/ddz.script $(1)/usr/sbin/hnetd-ddz-script
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/generic/pcp.script $(1)/usr/sbin/hnetd-pcp-script
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/generic/hnetd-routing $(1)/usr/sbin/hnetd-routing
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_BIN) ./files/hnetd.defaults $(1)/etc/uci-defaults/x-hnetd.defaults
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/generic/multicast.script $(1)/usr/sbin/hnet-multicast
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/openwrt/autowifi.script $(1)/usr/sbin/autowifi
endef

define Package/hnet-full/install
	true
endef

define Package/hnet-full-secure/install
	true
endef

define Package/hnet-full-l2tp/install
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/generic/tunnel.script $(1)/usr/sbin/hnetd-tunnel
endef

define Package/luci-app-hnet/install
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci
	$(INSTALL_DIR) $(1)/www
	$(CP) -R $(PKG_BUILD_DIR)/openwrt/luci/luasrc/* $(1)/usr/lib/lua/luci/
	$(CP) -R $(PKG_BUILD_DIR)/openwrt/luci/htdocs/* $(1)/www/
endef

define Package/hnetd-$(BUILD_VARIANT)/postinst
#!/bin/sh
[ -n "$${IPKG_INSTROOT}" ] || {
	(. /etc/uci-defaults/x-hnetd.defaults) && rm -f /etc/uci-defaults/x-hnetd.defaults
	[ -x /etc/init.d/dnsmasq ] && /etc/init.d/dnsmasq restart
	/etc/init.d/hnetd enable
	/etc/init.d/hnetd start
}
endef

#$(eval $(call BuildPackage,hnetd-nossl))
#$(eval $(call BuildPackage,hnetd-openssl))
#$(eval $(call BuildPackage,hnet-full))
#$(eval $(call BuildPackage,hnet-full-secure))
#$(eval $(call BuildPackage,hnet-full-l2tp))
#$(eval $(call BuildPackage,luci-app-hnet))
