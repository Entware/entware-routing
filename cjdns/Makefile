#
# Copyright (C) 2014,2015 Hyperboria.net
#
# You may redistribute this program and/or modify it under the terms of
# the GNU General Public License as published by the Free Software Foundation,
# either version 3 of the License, or (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=cjdns
PKG_VERSION:=0.20
PKG_RELEASE:=1

PKG_SOURCE_URL:=https://github.com/cjdelisle/cjdns.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=efd7d7f82be405fe47f6806b6cc9c0043885bc2e
PKG_LICENSE:=GPL-3.0
PKG_SOURCE:=$(PKG_NAME)-$(PKG_SOURCE_VERSION).tar.bz2
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_SOURCE_VERSION)
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_SOURCE_VERSION)

include $(INCLUDE_DIR)/package.mk


define Package/cjdns
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=Routing and Redirection
	TITLE:=Encrypted near-zero-conf mesh routing protocol
	URL:=https://github.com/cjdelisle/cjdns
	MAINTAINER:=Lars Gierth <larsg@systemli.org>
	DEPENDS:=+librt
endef

define Package/cjdns/description
	Cjdns implements an encrypted IPv6 network using public-key cryptography \
	for address allocation and a distributed hash table for routing. \
	This provides near-zero-configuration networking, and prevents many \
	of the security and scalability issues that plague existing networks.
endef

define Package/cjdns-tests
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=Routing and Redirection
	TITLE:=cjdns test cases
	URL:=https://github.com/cjdelisle/cjdns
	MAINTAINER:=Lars Gierth <larsg@systemli.org>
	DEPENDS:=+libpthread +librt
endef

define Package/cjdns-test/description
	cjdns test cases
endef

define Build/Configure
endef

PKG_DO_VARS:=CJDNS_RELEASE_VERSION=$(PKG_SOURCE_VERSION)


PKG_DO_VARS+= Seccomp_NO=1


ifneq ($(CONFIG_USE_UCLIBC),)
PKG_DO_VARS+= UCLIBC=1
endif

define Build/Compile
	$(INSTALL_DIR) $(PKG_BUILD_DIR)/tmp
	(cd $(PKG_BUILD_DIR) && \
	CROSS="true" \
	CC="$(TARGET_CC)" \
	AR="$(TARGET_AR)" \
	RANLIB="$(TARGET_RANLIB)" \
	CFLAGS="$(TARGET_CFLAGS) -Wno-shift-negative-value" \
	LDFLAGS="$(TARGET_LDFLAGS)" \
	SYSTEM="linux" \
	TARGET_ARCH="$(CONFIG_ARCH)" \
	SSP_SUPPORT="$(CONFIG_SSP_SUPPORT)" \
	GYP_ADDITIONAL_ARGS="-f make-linux" \
	CJDNS_BUILD_TMPDIR="$(PKG_BUILD_DIR)/tmp" \
	$(PKG_DO_VARS) \
	exec ./do)
endef

define Package/cjdns/install
	$(INSTALL_DIR) \
		$(1)/opt/sbin \
		$(1)/opt/bin

	$(INSTALL_BIN) \
		$(PKG_BUILD_DIR)/cjdroute \
		$(1)/opt/sbin

	$(INSTALL_BIN) \
		$(PKG_BUILD_DIR)/publictoip6 \
		$(1)/opt/bin
endef

define Package/cjdns-tests/install
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_BIN) \
		$(PKG_BUILD_DIR)/build_linux/test_testcjdroute_c \
		$(1)/opt/bin
endef

$(eval $(call BuildPackage,cjdns))
$(eval $(call BuildPackage,cjdns-tests))
