cmake_minimum_required(VERSION 3.5)
project(stb VERSION 0.8.1)

#########################################################################################
# STB Image
#########################################################################################
# Compile and export stb_image.h
option(STB_IMAGE ON)

# Suppress system IO interfacing code
option(STBI_NO_STDIO OFF)
# Suppress implementation of .hdr reader format
option(STBI_NO_HDR OFF)
# Suppress high-dynamic-range light-linear float API
option(STBI_NO_LINEAR OFF)

# You can suppress implementation of any of the decoders to reduce
# your code footprint by defining one or more of the following
# symbols before creating the implementation
option(STBI_NO_JPEG OFF)
option(STBI_NO_PNG OFF)
option(STBI_NO_BMP OFF)
option(STBI_NO_PSD OFF)
option(STBI_NO_TGA OFF)
option(STBI_NO_GIF OFF)
option(STBI_NO_HDR OFF)
option(STBI_NO_PIC OFF)
option(STBI_NO_PNM OFF) #(.ppm and .pgm)

# You can request *only* certain decoders and suppress all other ones
# (this will be more forward-compatible, as addition of new decoders
# doesn't require you to disable them explicitly):
option(STBI_ONLY_JPEG OFF)
option(STBI_ONLY_PNG OFF)
option(STBI_ONLY_BMP OFF)
option(STBI_ONLY_PSD OFF)
option(STBI_ONLY_TGA OFF)
option(STBI_ONLY_GIF OFF)
option(STBI_ONLY_HDR OFF)
option(STBI_ONLY_PIC OFF)
option(STBI_ONLY_PNM OFF) #(.ppm and .pgm)

# If you use STBI_NO_PNG (or _ONLY_ without PNG), and you still
# want the zlib decoder to be available, define STBI_SUPPORT_ZLIB.
option(STBI_SUPPORT_ZLIB OFF)

# Compilation of all SIMD code can be suppressed with
option(STBI_NO_SIMD OFF)

# If you must produce consistent results with previous versions of stb_image
option(STBI_JPEG_OLD OFF)

# STB Image target
if(STB_IMAGE)
    add_library(stb_image STATIC src/stb_image.c)
    target_include_directories(stb_image PUBLIC
        "$<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>")
    target_compile_definitions(stb_image PUBLIC 
        STB_IMAGE
        $<$<BOOL:${STBI_NO_STDIO}>:STBI_NO_STDIO>
        $<$<BOOL:${STBI_NO_HDR}>:STBI_NO_HDR>
        $<$<BOOL:${STBI_NO_LINEAR}>:STBI_NO_LINEAR>
        $<$<BOOL:${STBI_NO_JPEG}>:STBI_NO_JPEG>
        $<$<BOOL:${STBI_NO_PNG}>:STBI_NO_PNG>
        $<$<BOOL:${STBI_NO_BMP}>:STBI_NO_BMP>
        $<$<BOOL:${STBI_NO_PSD}>:STBI_NO_PSD>
        $<$<BOOL:${STBI_NO_TGA}>:STBI_NO_TGA>
        $<$<BOOL:${STBI_NO_GIF}>:STBI_NO_GIF>
        $<$<BOOL:${STBI_NO_HDR}>:STBI_NO_HDR>
        $<$<BOOL:${STBI_NO_PIC}>:STBI_NO_PIC>
        $<$<BOOL:${STBI_NO_PNM}>:STBI_NO_PNM>
        $<$<BOOL:${STBI_ONLY_JPEG}>:STBI_ONLY_JPEG>
        $<$<BOOL:${STBI_ONLY_PNG}>:STBI_ONLY_PNG>
        $<$<BOOL:${STBI_ONLY_BMP}>:STBI_ONLY_BMP>
        $<$<BOOL:${STBI_ONLY_PSD}>:STBI_ONLY_PSD>
        $<$<BOOL:${STBI_ONLY_TGA}>:STBI_ONLY_TGA>
        $<$<BOOL:${STBI_ONLY_GIF}>:STBI_ONLY_GIF>
        $<$<BOOL:${STBI_ONLY_HDR}>:STBI_ONLY_HDR>
        $<$<BOOL:${STBI_ONLY_PIC}>:STBI_ONLY_PIC>
        $<$<BOOL:${STBI_ONLY_PNM}>:STBI_ONLY_PNM>
        $<$<BOOL:${STBI_SUPPORT_ZLIB}>:STBI_SUPPORT_ZLIB>
        $<$<BOOL:${STBI_NO_SIMD}>:STBI_NO_SIMD>
        $<$<BOOL:${STBI_JPEG_OLD}>:STBI_JPEG_OLD>)

    install(TARGETS stb_image EXPORT stbTargets)
    install(FILES include/stb/stb_image.h DESTINATION include/stb)

endif(STB_IMAGE)


#########################################################################################
# Package Configuration
#########################################################################################
# Helpers functions for creating config files that can be included by other projects
include(CMakePackageConfigHelpers)

# Dummy target for generating export stbTargets even if no stb target was enabled
add_library(none INTERFACE)
install(TARGETS none EXPORT stbTargets)

# Create config package
configure_package_config_file(
    "Config.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/stbConfig.cmake"
    INSTALL_DESTINATION "lib/cmake/stb")
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/stbConfigVersion.cmake"
    COMPATIBILITY SameMajorVersion)

# Install config package
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/stbConfig.cmake"
              "${CMAKE_CURRENT_BINARY_DIR}/stbConfigVersion.cmake"
        DESTINATION "lib/cmake/stb")
install(EXPORT "stbTargets"
        NAMESPACE "stb::"
        DESTINATION "lib/cmake/stb")
