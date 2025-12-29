# USB Device UVC is only supported on ESP32-S2, ESP32-S3, and ESP32-P4
if(IDF_TARGET STREQUAL "esp32s2" OR IDF_TARGET STREQUAL "esp32s3" OR IDF_TARGET STREQUAL "esp32p4")
    idf_component_register(SRCS usb_device_uvc.c
                        INCLUDE_DIRS "include"
                        REQUIRES usb esp_timer)

    idf_component_get_property(tusb_lib espressif__tinyusb COMPONENT_LIB)

    target_include_directories(${tusb_lib} PUBLIC "${COMPONENT_DIR}/tusb")
    target_sources(${tusb_lib} PUBLIC "${COMPONENT_DIR}/tusb/usb_descriptors.c")

    include(package_manager)
    cu_pkg_define_version(${CMAKE_CURRENT_LIST_DIR})
else()
    # Register empty component for unsupported targets
    idf_component_register()
endif()
