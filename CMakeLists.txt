cmake_minimum_required(VERSION 3.18)
set(CMAKE_TOOLCHAIN_FILE ${CMAKE_CURRENT_SOURCE_DIR}/vcpkg/scripts/buildsystems/vcpkg.cmake CACHE STRING "Vcpkg toolchain file")

project(__MY_APP__)

# build type
set(default_build_type "Debug")
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "Setting build type to '${default_build_type}' as none was specified.")
  set(CMAKE_BUILD_TYPE "${default_build_type}" CACHE
      STRING "Choose the type of build." FORCE)
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
    "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

# output paths
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# write compile_commands.json file
set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)

# compile flags
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -march=native")
set(SANITIZE_FLAGS "-fsanitize=address,leak,undefined -fno-common -fno-omit-frame-pointer")
set(CMAKE_CXX_FLAGS_DEBUG "-Wall -Wextra -O0 ${SANITIZE_FLAGS}")

# 3rd party libs
find_package(fmt CONFIG REQUIRED)

set(SRC
    src/main.cpp
    )

add_executable(__MY_APP__ ${SRC})
set_property(TARGET __MY_APP__ PROPERTY CXX_STANDARD 17)
target_link_libraries(__MY_APP__ PRIVATE fmt::fmt)
