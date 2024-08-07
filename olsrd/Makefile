# SPDX-License-Identifier: GPL-2.0-or-later
#
# Copyright (C) 2009-2016 OpenWrt.org
#

include $(TOPDIR)/rules.mk

PKG_NAME:=olsrd
PKG_SOURCE_DATE:=2024-06-09
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/OLSR/olsrd.git
PKG_SOURCE_VERSION:=d72be9ad60f65dda28a0543b4ffff8cd9a5da2d7
PKG_MIRROR_HASH:=e05d2724e6c76233148ce460b9611bafccff2114904542fa3b5caf75dc1c4e04

PKG_MAINTAINER:=Nick Hainke <vincent@systemli.org>
PKG_BUILD_PARALLEL:=0
PKG_LICENSE:=BSD-3-Clause

include $(INCLUDE_DIR)/package.mk

TARGET_CFLAGS += $(FPIC)

define Package/olsrd/template
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Routing and Redirection
  MAINTAINER:=Saverio Proto <zioproto@gmail.com>
  TITLE:=OLSR (Optimized Link State Routing) daemon
  URL:=http://www.olsr.org/
endef

define Package/olsrd
  $(call Package/olsrd/template)
  MENU:=1
  DEPENDS:=+libpthread +iptables +IPV6:iptables
endef

#define Package/olsrd/conffiles
#/etc/config/olsrd
#/etc/config/olsrd6
#endef

define Package/olsrd-mod-arprefresh
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Kernel ARP cache refresh plugin
endef

define Package/olsrd-mod-dot-draw
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Dot topology information plugin
endef


define Package/olsrd-mod-filtergw
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Filter Gateway plugin
endef

define Package/olsrd-mod-bmf
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Basic multicast forwarding plugin
endef

define Package/olsrd-mod-dyn-gw
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Dynamic internet gateway plugin
endef

define Package/olsrd-mod-dyn-gw-plain
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Dynamic internet gateway plain plugin
endef

define Package/olsrd-mod-httpinfo
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Small informative web server plugin
endef

define Package/olsrd-mod-jsoninfo
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Small informative plugin with JSON output
endef

define Package/olsrd-mod-netjson
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=netjson plugin with JSON output
endef

define Package/olsrd-mod-mdns
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=MDNS/Zeroconf/Bonjour packet distribution
endef

define Package/olsrd-mod-nameservice
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Lightweight hostname resolver plugin
endef

define Package/olsrd-mod-p2pd
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Peer to Peer Discovery plugin
endef

define Package/olsrd-mod-pgraph
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=output network topology for pgraph
endef

define Package/olsrd-mod-pud
  $(call Package/olsrd/template)
  DEPENDS:=olsrd +libgps
  TITLE:=Position Update Distribution plugin
endef

define Package/olsrd-mod-quagga
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Quagga plugin
endef

define Package/olsrd-mod-secure
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Message signing plugin to secure routing domain
endef

define Package/olsrd-mod-sgwdynspeed
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Smart Gateway dynamic speed plugin
endef

define Package/olsrd-mod-txtinfo
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Small informative web server plugin
endef

define Package/olsrd-mod-watchdog
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Watchdog plugin
endef

define Package/olsrd-utils
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Utils for OLSRD
endef

define Package/olsrd-mod-pud/conffiles
/opt/etc/olsrd.d/olsrd.pud.position.conf
endef

define Package/olsrd-mod-secure/conffiles
/opt/etc/olsrd.d/olsrd_secure_key
endef

define Package/olsrd-mod-sgwdynspeed/conffiles
/opt/etc/olsrd.d/olsrd.sgw.speed.conf
endef

MAKE_FLAGS+= \
	EXTRA_CPPFLAGS="$(TARGET_CPPFLAGS)" \
	NO_DEBUG_MESSAGES=1 \
	OS="linux" \
	DESTDIR="$(PKG_INSTALL_DIR)" \
	STRIP="true" \
	INSTALL_LIB="true" \
	SUBDIRS="arprefresh bmf dot_draw dyn_gw dyn_gw_plain httpinfo filtergw jsoninfo netjson mdns nameservice p2pd pgraph pud quagga secure sgwdynspeed txtinfo watchdog"

define Build/Compile
	$(call Build/Compile/Default,all)
	$(call Build/Compile/Default,libs)
endef

define Package/olsrd/install
#	$(INSTALL_DIR) $(1)/lib/functions
#	$(INSTALL_DATA) ./files/olsrd.sh $(1)/lib/functions/olsrd.sh
#	$(INSTALL_DIR) $(1)/etc/config
#	$(INSTALL_DATA) ./files/olsrd.config $(1)/etc/config/olsrd
#	$(INSTALL_DATA) ./files/olsrd6.config $(1)/etc/config/olsrd6
	$(INSTALL_DIR) $(1)/opt/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/olsrd $(1)/opt/sbin/
#	$(INSTALL_DIR) $(1)/etc/init.d
#	$(INSTALL_BIN) ./files/olsrd4.init $(1)/etc/init.d/olsrd
#	$(INSTALL_BIN) ./files/olsrd6.init $(1)/etc/init.d/olsrd6
endef

define Package/olsrd-mod-arprefresh/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/arprefresh/olsrd_arprefresh.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-filtergw/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/filtergw/olsrd_filtergw.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-dot-draw/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/dot_draw/olsrd_dot_draw.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-bmf/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/bmf/olsrd_bmf.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-dyn-gw/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/dyn_gw/olsrd_dyn_gw.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-dyn-gw-plain/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/dyn_gw_plain/olsrd_dyn_gw_plain.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-httpinfo/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/httpinfo/olsrd_httpinfo.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-jsoninfo/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/jsoninfo/olsrd_jsoninfo.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-netjson/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/netjson/olsrd_netjson.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-mdns/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/mdns/olsrd_mdns.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-nameservice/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/nameservice/olsrd_nameservice.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-p2pd/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/p2pd/olsrd_p2pd.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-pgraph/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/pgraph/olsrd_pgraph.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-pud/install
	$(INSTALL_DIR) $(1)/opt/etc/olsrd.d
	$(CP) ./files/olsrd.pud.position.conf $(1)/opt/etc/olsrd.d/
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/pud/nmealib/lib/libnmea.so $(1)/opt/lib/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/pud/wireformat/lib/libOlsrdPudWireFormat.so $(1)/opt/lib/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/pud/olsrd_pud.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-quagga/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/quagga/olsrd_quagga.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-secure/install
	$(INSTALL_DIR) $(1)/opt/etc/olsrd.d
	$(CP) ./files/olsrd_secure_key $(1)/opt/etc/olsrd.d/
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/secure/olsrd_secure.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-sgwdynspeed/install
	$(INSTALL_DIR) $(1)/opt/etc/olsrd.d
	$(CP) ./files/olsrd.sgw.speed.conf $(1)/opt/etc/olsrd.d/
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/sgwdynspeed/olsrd_sgwdynspeed.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-txtinfo/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/txtinfo/olsrd_txtinfo.so.* $(1)/opt/lib/
endef

define Package/olsrd-mod-watchdog/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/watchdog/olsrd_watchdog.so.* $(1)/opt/lib/
endef

define Package/olsrd-utils/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) ./files/olsrd-neigh.sh $(1)/usr/bin/olsrd-neigh
endef

$(eval $(call BuildPackage,olsrd))
$(eval $(call BuildPackage,olsrd-mod-arprefresh))
$(eval $(call BuildPackage,olsrd-mod-dot-draw))
$(eval $(call BuildPackage,olsrd-mod-filtergw))
$(eval $(call BuildPackage,olsrd-mod-bmf))
$(eval $(call BuildPackage,olsrd-mod-dyn-gw))
$(eval $(call BuildPackage,olsrd-mod-dyn-gw-plain))
$(eval $(call BuildPackage,olsrd-mod-httpinfo))
$(eval $(call BuildPackage,olsrd-mod-jsoninfo))
$(eval $(call BuildPackage,olsrd-mod-netjson))
$(eval $(call BuildPackage,olsrd-mod-mdns))
$(eval $(call BuildPackage,olsrd-mod-nameservice))
$(eval $(call BuildPackage,olsrd-mod-p2pd))
$(eval $(call BuildPackage,olsrd-mod-pgraph))
$(eval $(call BuildPackage,olsrd-mod-pud))
$(eval $(call BuildPackage,olsrd-mod-quagga))
$(eval $(call BuildPackage,olsrd-mod-secure))
$(eval $(call BuildPackage,olsrd-mod-sgwdynspeed))
$(eval $(call BuildPackage,olsrd-mod-txtinfo))
$(eval $(call BuildPackage,olsrd-mod-watchdog))
#$(eval $(call BuildPackage,olsrd-utils))
