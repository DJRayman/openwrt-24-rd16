include $(TOPDIR)/rules.mk

PKG_NAME:=qca-wpa-cli
PKG_VERSION:=2022-07-29
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://w1.fi/hostap.git
PKG_SOURCE_DATE:=2022-07-29
PKG_SOURCE_VERSION:=b704dc72ef824dfdd96674b90179b274d1d38105
PKG_MIRROR_HASH:=6c9dd359ef5a4595b6576e07928566d6864957c4af6466d641d6c3f7717f4689

PKG_BUILD_DIR:=$(BUILD_DIR)/hostapd-wpa-cli-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/qca-wpa-cli
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Wireless
  TITLE:=Qualcomm wpa_cli utility and control scripts
  DEPENDS:=@TARGET_ipq53xx
endef

define Package/qca-wpa-cli/description
  Native wpa_cli utility with Qualcomm-specific extensions and scripts
  (WPS button handlers and UCI helpers) for IPQ53xx platforms.
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	tar -xf $(DL_DIR)/$(PKG_SOURCE) --strip-components=1 -C $(PKG_BUILD_DIR)
	$(Build/Patch)
endef

define Build/Configure
	cp $(PKG_BUILD_DIR)/wpa_supplicant/defconfig $(PKG_BUILD_DIR)/wpa_supplicant/.config
	echo "CONFIG_PMKSA_CACHE_EXTERNAL=y" >> $(PKG_BUILD_DIR)/wpa_supplicant/.config
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)/wpa_supplicant \
		$(TARGET_CONFIGURE_OPTS) \
		EXTRA_CFLAGS="$(TARGET_CFLAGS)" \
		LIBS="$(TARGET_LDFLAGS)" \
		wpa_cli
endef

define Package/qca-wpa-cli/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/wpa_supplicant/wpa_cli $(1)/usr/sbin/
	$(if $(wildcard ./files/*),$(CP) ./files/* $(1)/)
endef

$(eval $(call BuildPackage,qca-wpa-cli))
