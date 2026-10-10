include $(TOPDIR)/rules.mk

PKG_NAME:=qca-hostapd-cli
PKG_RELEASE:=1

PKG_SOURCE_URL:=https://w1.fi/hostap.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2022-07-29
PKG_SOURCE_VERSION:=b704dc72ef824dfdd96674b90179b274d1d38105
PKG_MIRROR_HASH:=6c9dd359ef5a4595b6576e07928566d6864957c4af6466d641d6c3f7717f4689

PKG_MAINTAINER:=Qualcomm
PKG_LICENSE:=BSD-3-Clause

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/qca-hostapd-cli
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Wireless
  TITLE:=Qualcomm hostapd_cli utility and control scripts
  DEPENDS:=@TARGET_ipq53xx
endef

define Package/qca-hostapd-cli/description
  Native hostapd_cli utility with Qualcomm-specific extensions and scripts
  (hapd, wpsd, wps and dpp helpers) for IPQ53xx platforms.
endef

define Build/Configure
	cp $(PKG_BUILD_DIR)/hostapd/defconfig $(PKG_BUILD_DIR)/hostapd/.config
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)/hostapd \
		$(TARGET_CONFIGURE_OPTS) \
		EXTRA_CFLAGS="$(TARGET_CFLAGS)" \
		LIBS="$(TARGET_LDFLAGS)" \
		hostapd_cli
endef

define Package/qca-hostapd-cli/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/hostapd/hostapd_cli $(1)/usr/sbin/
	$(if $(wildcard ./files/*),$(CP) ./files/* $(1)/)
endef

$(eval $(call BuildPackage,qca-hostapd-cli))
