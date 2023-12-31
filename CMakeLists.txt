cmake_minimum_required(VERSION 3.10)

project(adcrf VERSION 0.1)

include(GNUInstallDirs)

if(NOT SYSTEMD_UNIT_PATH)
    set(SYSTEMD_UNIT_PATH "/lib/systemd/system/")
endif()

message(STATUS "Systemd service file installation path have been set to: " ${SYSTEMD_UNIT_PATH})

add_executable(adcrf 
    src/rpi_adc_stream.c
    src/rpi_dma_utils.c
)

target_include_directories(adcrf 
    PRIVATE 
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
)

install(TARGETS adcrf DESTINATION ${CMAKE_INSTALL_BINDIR})
install(FILES conf/adcrf.service DESTINATION ${SYSTEMD_UNIT_PATH})

set(CPACK_PACKAGE_NAME "adcrf")
set(CPACK_PACKAGE_VENDOR "Mikhail Tcukerman <michael@zuckerman.dev>")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Analog to digital convertion reader DMA sample app")
set(CPACK_PACKAGE_VERSION "${PROJECT_VERSION}")
set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER ${CPACK_PACKAGE_VENDOR})
set(CPACK_GENERATOR "RPM;DEB")

include(CPack)
