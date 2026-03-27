# SPDX-License-Identifier: GPL-2.0-only

include $(TOPDIR)/rules.mk

PKG_NAME:=phantun
PKG_VERSION:=0.8.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/dndx/phantun/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=fc435fa909f5410c482bde89d3b03b42a21f4e6314595400cf010c272ef8c023

PKG_MAINTAINER:=OpenWrt Packager
PKG_LICENSE:=MIT Apache-2.0
PKG_LICENSE_FILES:=LICENSE-MIT LICENSE-APACHE
PKG_BUILD_DEPENDS:=rust/host
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

RUST_PACKAGE_MK:=$(TOPDIR)/feeds/packages/lang/rust/rust-package.mk
ifneq ($(wildcard $(RUST_PACKAGE_MK)),)
  include $(RUST_PACKAGE_MK)
else
  include ../../lang/rust/rust-package.mk
endif

define Package/phantun
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=VPN
  TITLE:=UDP-to-fake-TCP tunnel for firewall/NAT traversal
  URL:=https://github.com/dndx/phantun
  DEPENDS:=$(RUST_ARCH_DEPENDS) +kmod-tun +ip-full
endef

define Package/phantun/description
  Phantun transforms UDP streams into fake TCP streams to pass through
  layer 3/4 firewalls and NAT.
endef

define Package/phantun/conffiles
/etc/config/phantun
endef

define Package/phantun/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/phantun_client $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/phantun_server $(1)/usr/bin/

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) $(CURDIR)/files/phantun.config $(1)/etc/config/phantun

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(CURDIR)/files/phantun.init $(1)/etc/init.d/phantun
endef

$(eval $(call BuildPackage,phantun))
