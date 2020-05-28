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
PKG_VERSION:=v20.4
PKG_RELEASE:=3

PKG_SOURCE_VERSION:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/cjdelisle/cjdns.git
PKG_MIRROR_HASH:=5e0036660626c3acf0050cf2e47aa87d262474184efb9d69538dbab4512113f8

PKG_MAINTAINER:=William Fleurant <meshnet@protonmail.com>
PKG_LICENSE:=GPL-3.0-or-later
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/cjdns
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=Routing and Redirection
	TITLE:=Encrypted near-zero-conf mesh routing protocol
	URL:=https://github.com/cjdelisle/cjdns
	DEPENDS:=@!arc @IPV6 +libpthread +librt
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
	DEPENDS:=+libpthread +librt @!arc
endef

define Package/cjdns-test/description
	Builds cjdns test cases binary test_testcjdroute_c
endef

define Build/Configure
endef

PKG_DO_VARS:=CJDNS_RELEASE_VERSION=$(PKG_SOURCE_VERSION)

#ifneq ($(CONFIG_KERNEL_SECCOMP_FILTER),y)
PKG_DO_VARS+= Seccomp_NO=1
#endif

ifneq ($(CONFIG_USE_UCLIBC),)
PKG_DO_VARS+= UCLIBC=1
endif

ifeq ($(or $(CONFIG_GCC_USE_VERSION_8),$(CONFIG_GCC_USE_VERSION_9)),y)
TARGET_CFLAGS += -Wno-error=stringop-truncation -Wno-error=cpp -Wno-error=deprecated-declarations
endif

define Build/Compile
	$(INSTALL_DIR) $(BUILD_DIR)/$(PKG_SOURCE_VERSION)/tmp
	(cd $(BUILD_DIR)/$(PKG_SOURCE_VERSION) && \
	CROSS="true" \
	CC="$(TARGET_CC)" \
	AR="$(TARGET_AR)" \
	RANLIB="$(TARGET_RANLIB)" \
	CFLAGS="$(TARGET_CFLAGS) -U_FORTIFY_SOURCE" \
	LDFLAGS="$(TARGET_LDFLAGS)" \
	SYSTEM="linux" \
	TARGET_ARCH="$(CONFIG_ARCH)" \
	SSP_SUPPORT="$(CONFIG_SSP_SUPPORT)" \
	GYP_ADDITIONAL_ARGS="-f make-linux" \
	CJDNS_BUILD_TMPDIR="$(BUILD_DIR)/$(PKG_SOURCE_VERSION)/tmp" \
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
		$(BUILD_DIR)/$(PKG_SOURCE_VERSION)/build_linux/test_testcjdroute_c \
		$(1)/opt/bin
endef

$(eval $(call BuildPackage,cjdns))
$(eval $(call BuildPackage,cjdns-tests))
