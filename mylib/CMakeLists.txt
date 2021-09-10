cmake_minimum_required(VERSION 3.10)
project(MyLib)

# for vscode
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(PACKAGE_NAME MyLib)
set(INCLUDE_INSTALL_DIR ${PACKAGE_NAME}/include)
set(LIBRARY_INSTALL_DIR lib)
set(BIN_INSTALL_DIR bin)
set(CONFIG_FILE_INSTALL_DIR "lib/cmake/${PACKAGE_NAME}")

# make install 安装目录
set(CMAKE_INSTALL_PREFIX "/home/cxn/code/cplus/install")
# 安装package（如 dpkg -i xxx.deb）时的安装目录
set(CPACK_PACKAGING_INSTALL_PREFIX "/home/cxn/code/cplus/package")

add_subdirectory(myliba)
add_subdirectory(mylibb)

add_executable(example ${CMAKE_CURRENT_SOURCE_DIR}/example.cpp)
target_link_libraries(example PRIVATE libfuna libfunb)

# 将 example (执行程序) 安装在 ${BIN_INSTALL_DIR}目录下
# make install时安装目录为：${CMAKE_INSTALL_PREFIX}/${BIN_INSTALL_DIR}
# package安装时目录为：${CMAKE_INSTALL_PREFIX}/${BIN_INSTALL_DIR}
install(
  TARGETS example
  DESTINATION ${BIN_INSTALL_DIR}
  EXPORT ExampleTargets)
install(EXPORT ExampleTargets DESTINATION ${BIN_INSTALL_DIR}/cmake/example)

# CTEST#####
enable_testing()
add_test(NAME test_one COMMAND example 1 2 3)
add_test(NAME test_two COMMAND example 1 2 3)

# CPACK

# set a project version
set(MyLib_VERSION_MAJOR 0)
set(MyLib_VERSION_MINOR 2)
set(MyLib_VERSION_PATCH 2)
set(MyLib_VERSION
    "${MyLib_VERSION_MAJOR}.${MyLib_VERSION_MINOR}.${MyLib_VERSION_PATCH}")
# Tell CPack to generate a .deb package
set(CPACK_GENERATOR "DEB")
# Set a Package Maintainer.This is required
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "Leonard")
# Set a Package Version
set(CPACK_PACKAGE_VERSION ${MyLib_VERSION})
# Include CPack
include(CPack)

# 生成MyLibConfig.cmake与MyLibConfigVersion.cmake，使其他人能够调用这个库
include(CMakePackageConfigHelpers)

# 将${PACKAGE_NAME}Config.cmake.in的内容复制到${PACKAGE_NAME}Config.cmake中
# PACKAGE_NAME INCLUDE_INSTALL_DIR LIBRARY_INSTALL_DIR BIN_INSTALL_DIR这四个变量是 {PACKAGE_NAME}Config.cmake.in 可能会用到的
# INSTALL_DESTINATION 干啥用的不清楚？
configure_package_config_file(
  ${CMAKE_SOURCE_DIR}/${PACKAGE_NAME}Config.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/${PACKAGE_NAME}Config.cmake
  INSTALL_DESTINATION "${CONFIG_FILE_INSTALL_DIR}"
  PATH_VARS PACKAGE_NAME INCLUDE_INSTALL_DIR LIBRARY_INSTALL_DIR
            BIN_INSTALL_DIR)

# 生成M${PACKAGE_NAME}ConfigVersion.cmake
write_basic_package_version_file(
  ${PACKAGE_NAME}ConfigVersion.cmake
  VERSION ${CPACK_PACKAGE_VERSION}
  COMPATIBILITY AnyNewerVersion # 表示该函数库向下兼容
)

# 安装MyLibConfig.cmake与MyLibConfigVersion.cmake
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/${PACKAGE_NAME}Config.cmake"
              "${CMAKE_CURRENT_BINARY_DIR}/${PACKAGE_NAME}ConfigVersion.cmake"
        DESTINATION ${CONFIG_FILE_INSTALL_DIR})
