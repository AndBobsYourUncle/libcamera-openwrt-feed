# Copyright (C) 2024 Fusion CCTV, Inc.

include $(TOPDIR)/rules.mk

PKG_NAME:=libcamera
PKG_VERSION:=0.6.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/raspberrypi/libcamera.git
PKG_SOURCE_DATE:=2025-12-02
PKG_SOURCE_VERSION:=f0e40f1c50bd0afe65727d6e407d0dcb42666ada
PKG_MAINTAINER:= Marcus Folkesson <marcus.folkesson@gmail.com>
PKG_LICENSE:= \
	LGPL-2.1+ (library), \
	GPL-2.0+ (utils), \
	MIT (qcam/assets/feathericons), \
	BSD-2-Clause (raspberrypi), \
	GPL-2.0 with Linux-syscall-note or BSD-3-Clause (linux kernel headers), \
	CC0-1.0 (meson build system), \
	CC-BY-SA-4.0 (doc)

PKG_BUILD_DEPENDS:= \
	python3/host \
	python-jinja2/host \
	python-ply/host \
	python-yaml/host \
	openssl \
	libevent2 \
	LIBCAMERA_GSTREAMER_SUPPORT:gstreamer1-libs \
	LIBCAMERA_GSTREAMER_SUPPORT:gst1-plugins-base

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/meson.mk


define Package/libcamera/config
  source "$(SOURCE)/Config.in"
endef

define Package/libcamera/default
	SECTION:=libs
	CATEGORY:=Multimedia
	DEPENDS:=+libyaml +libgnutls +libstdcpp +libudev-zero +libevent2 +libevent2-pthreads +libdrm $(if $(CONFIG_LIBCAMERA_GSTREAMER_SUPPORT),+gst1-plugins-base +gstreamer1-libs)
	TITLE:=libcamera
	URL:=https://libcamera.org/
endef

define Package/libcamera
	$(call Package/libcamera/default)
endef

define Package/libcamera/description
	libcamera provides a software stack to support
	complex devices that need heavy hardware image
	processing operations.

	http://www.libcamera.org/
endef

ifeq ($(CONFIG_LIBCAMERA_PIPELINE_IMX8_ISI),y)
	LIBCAMERA_PIPELINES += imx8-isi
endif
ifeq ($(CONFIG_LIBCAMERA_PIPELINE_IPU3),y)
	LIBCAMERA_PIPELINES += ipu3
endif
ifeq ($(CONFIG_LIBCAMERA_PIPELINE_RKISP1),y)
	LIBCAMERA_PIPELINES += rkisp1
endif
ifeq ($(CONFIG_LIBCAMERA_PIPELINE_RPI_VC4),y)
	LIBCAMERA_PIPELINES += rpi/vc4
endif
ifeq ($(CONFIG_LIBCAMERA_PIPELINE_SIMPLE),y)
	LIBCAMERA_PIPELINES += simple
endif
ifeq ($(CONFIG_LIBCAMERA_PIPELINE_VIMC),y)
	LIBCAMERA_PIPELINES += vimc
endif
MESON_ARGS += -Dpipelines=$(subst $(space),$(comma),$(LIBCAMERA_PIPELINES))

MESON_ARGS += -Dv4l2=$(if $(CONFIG_LIBCAMERA_V4L2),enabled,disabled)
MESON_ARGS += -Dgstreamer=$(if $(CONFIG_LIBCAMERA_GSTREAMER_SUPPORT),enabled,disabled)

MESON_ARGS += \
	-Dandroid=disabled \
	-Ddocumentation=disabled \
	-Dtest=false \
	-Dpycamera=disabled \
	-Dwerror=false \
	-Dcam=enabled \
	-Dqcam=disabled \
	-Dstrip=false

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/lib*.so* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/*.pc $(1)/usr/lib/pkgconfig
endef

define Package/libcamera/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/cam $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/lib*.so.* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/share/libcamera
	$(CP) $(PKG_INSTALL_DIR)/usr/share/libcamera/* $(1)/usr/share/libcamera/
	$(INSTALL_DIR) $(1)/usr/libexec/libcamera
	$(CP) $(PKG_INSTALL_DIR)/usr/libexec/libcamera/* $(1)/usr/libexec/libcamera/
	$(INSTALL_DIR) $(1)/usr/lib/libcamera/ipa
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libcamera/ipa/* $(1)/usr/lib/libcamera/ipa/
	$(INSTALL_DIR) $(1)/usr/share/libcamera/ipa
	$(CP) $(PKG_INSTALL_DIR)/usr/share/libcamera/ipa/* $(1)/usr/share/libcamera/ipa/
ifeq ($(CONFIG_LIBCAMERA_GSTREAMER_SUPPORT),y)
	$(INSTALL_DIR) $(1)/usr/lib/gstreamer-1.0
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/gstreamer-1.0/libgstlibcamera.so* $(1)/usr/lib/gstreamer-1.0/
endif
endef


$(eval $(call BuildPackage,libcamera))
