include $(TOPDIR)/rules.mk

PKG_NAME:=usteer-ng
PKG_SOURCE_PROTO:=git

# Resolve the repository root dynamically so this also works when the checkout layout differs.
PKG_GIT_REPO_ROOT:=$(shell git -C $(CURDIR) rev-parse --show-toplevel 2>/dev/null)

# Get current commit
PKG_COMMIT:=$(shell git -C $(PKG_GIT_REPO_ROOT) rev-parse HEAD 2>/dev/null)

# Use commit date for an APK-compatible package version
PKG_VERSION:=$(shell git -C $(PKG_GIT_REPO_ROOT) show -s --format=%cd --date=format:%Y.%m.%d $(PKG_COMMIT) 2>/dev/null)

# Upstream repository
PKG_SOURCE_URL:=https://github.com/NilsRo/usteer-ng.git
PKG_SOURCE_PROTO:=git

PKG_LATEST_TAG:=$(shell git ls-remote --tags --refs $(PKG_SOURCE_URL) | awk '{print $$2}' | sed 's|refs/tags/||' | sort -V | tail -n 1)

# PKG_SOURCE_VERSION:=$(shell git ls-remote $(PKG_SOURCE_URL) refs/tags/$(PKG_LATEST_TAG) | awk '{print $$1}')

# PKG_VERSION:=$(patsubst usteer-ng-%,%,$(PKG_LATEST_TAG))
# PKG_RELEASE:=1

PKG_SOURCE_VERSION:=1.0.2
PKG_VERSION:=1.0.2
PKG_RELEASE:=1

PKG_HASH:=skip
PKG_SOURCE_HASH:=skip
PKG_MIRROR_HASH:=skip

$(info ===== usteer-ng version diagnostics =====)
$(info PKG_SOURCE_URL='$(PKG_SOURCE_URL)')
$(info PKG_LATEST_TAG='$(PKG_LATEST_TAG)')
$(info PKG_SOURCE_VERSION='$(PKG_SOURCE_VERSION)')
$(info PKG_VERSION='$(PKG_VERSION)')
$(info ========================================)

PKG_MAINTAINER:=Nils Hendrik Rottgardt <n.rottgardt@gmail.com>
PKG_LICENSE:=GPL-2.0-only

PKG_BUILD_DEPENDS:=libpcap
PKG_BUILD_PARALLEL:=1

PKG_FILE_DEPENDS:=$(CURDIR)/../..

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	ln -s $(CURDIR)/../../.git $(PKG_BUILD_DIR)/.git
	cd $(PKG_BUILD_DIR) && git checkout .
endef

define Package/usteer-ng
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Wireless
  DEPENDS:=+libubox +libubus +libblobmsg-json +libnl-tiny
  TITLE:=Usteer - Next Generation OpenWrt AP roaming assist daemon
  PROVIDES:=usteer $(PKG_NAME)
  REPLACES:=usteer
  URL:=https://github.com/NilsRo/usteer-ng/
endef

define Package/usteer-ng/conffiles
/etc/config/usteer
endef

define Package/usteer-ng/install
	$(INSTALL_DIR) $(1)/sbin $(1)/etc/init.d $(1)/etc/config
	$(CP) ./files/* $(1)/
	$(CP) $(PKG_BUILD_DIR)/usteerd $(1)/sbin/
endef

$(eval $(call BuildPackage,usteer-ng))
