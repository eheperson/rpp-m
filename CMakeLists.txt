# Change version of the cmake accorting of your   <REQUIRED>
cmake_minimum_required(VERSION 3.22.2 FATAL_ERROR)

# set the project name with version
# ${PROJECT_NAME}
project(rppm VERSION 1.0.0)

# Specify C++ standards
# C++ standards declerations must be above the call to add_executable
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Specify C Standards
# C standards declerations must be above the call to add_executable
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED True)
set(CMAKE_C_EXTENSIONS ON)

# ObjectiveC Standards
if(APPLE)
    enable_language(OBJC)
endif()

#------------------------------------------------------------------------------------------------------------
# RPATH Settings

set(CMAKE_MACOSX_RPATH 1)

# # use, i.e. don't skip the full RPATH for the build tree
set(CMAKE_SKIP_BUILD_RPATH FALSE)

# # when building, don't use the install RPATH already
# # (but later on when installing)
set(CMAKE_BUILD_WITH_INSTALL_RPATH FALSE)

set(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib")

# # add the automatically determined parts of the RPATH
# # which point to directories outside the build tree to the install RPATH
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)
set(INSTALL_RPATH_USE_LINK_PATH TRUE)

# # the RPATH to be used when installing, but only if it's not a system directory
list(FIND CMAKE_PLATFORM_IMPLICIT_LINK_DIRECTORIES "${CMAKE_INSTALL_PREFIX}/lib" isSystemDir)
if("${isSystemDir}" STREQUAL "-1")
    set(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib")
endif("${isSystemDir}" STREQUAL "-1")
#------------------------------------------------------------------------------------------------------------


# Configure the header file to pass the version number to the source code
configure_file(rppmConfig.h.in rppmConfig.h)

# add the executable
add_executable(${CMAKE_PROJECT_NAME} main.cpp)

# add_executable(temp temp.cpp) #example to add new executable : Step1
# target_include_directories(temp PUBLIC "${PROJECT_BINARY_DIR}") #example to add new executable : Step2

# add the binary tree to the search path for include files# so that we will find rppfConfig.h
target_include_directories(${PROJECT_NAME} PUBLIC ${PROJECT_BINARY_DIR})


# ---------------------- Installing Begin ----------------------
# add the install targets
install(TARGETS ${CMAKE_PROJECT_NAME} DESTINATION bin)
# install(TARGETS temp DESTINATION bin) #example to add new executable : Step3 (installation)
install(FILES "${PROJECT_BINARY_DIR}/rppmConfig.h"DESTINATION include)
# ---------------------- Installing End ------------------------


# -------------Adding Uninstall Support Begin -------------------
# (experimental)
add_custom_target("uninstall" COMMENT "Uninstall installed files")
add_custom_command(
    TARGET "uninstall"
    POST_BUILD
    COMMENT "Uninstall files with install_manifest.txt"
    COMMAND xargs rm -vf < install_manifest.txt || echo Nothing in
            install_manifest.txt to be uninstalled!
)
# -------------Adding Uninstall Support End ---------------------


# ---------------------- Packing Begin ----------------------
# Options for packing
set(PACK_PACKAGE_VENDOR "eheperson")
set(CPACK_SYSTEM_NAME "linux")
set(PACKAGE_DIR "${CMAKE_SOURCE_DIR}/out/package")
set(CPACK_PACKAGE_DIRECTORY "${PACKAGE_DIR}")
#
# to distribute our project to other people 
# This module will include any runtime libraries that are needed by the project 
# for the current platform
include(InstallRequiredSystemLibraries)
#
# set some CPack variables to where we have stored the license and 
# version information for this project.
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "My funky project")
set(CPACK_PACKAGE_VENDOR ${CPACK_PACKAGE_VENDOR})
#
set(CPACK_PACKAGE_DESCRIPTION_FILE "${CMAKE_CURRENT_SOURCE_DIR}/README.md")
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_CURRENT_SOURCE_DIR}/License.txt")
#
set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_NAME_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_NAME_VERSION_MINOR}")
# #
set(CPACK_SOURCE_GENERATOR "TGZ")
# #
include(CPack)
# ---------------------- Packing End ------------------------
