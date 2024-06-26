cmake_minimum_required(VERSION 3.16)

add_library(bsatk STATIC)
mo2_configure_library(bsatk
    WARNINGS OFF PERMISSIVE ON
    PUBLIC_DEPENDS boost boost::thread
    PRIVATE_DEPENDS zlib lz4)
target_include_directories(bsatk PUBLIC ${DDS_ROOT})
mo2_install_target(bsatk)
