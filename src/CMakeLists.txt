cmake_minimum_required(VERSION 3.16)

add_library(bsatk STATIC)
mo2_configure_library(bsatk
    WARNINGS OFF PERMISSIVE ON
    PUBLIC_DEPENDS boost boost::thread
    PRIVATE_DEPENDS DirectXTex zlib lz4)
mo2_install_target(bsatk)
