cmake_minimum_required(VERSION 3.10)
project(libfuna)
add_library(libfuna SHARED ${CMAKE_CURRENT_SOURCE_DIR}/src/liba.cpp)

# 使用到CPACK使就要注意：BUILD_INTERFACE用于生成库时的编译
# 但是INSTALL_INTERFACE就不知道是怎么个用法了？
target_include_directories(
  libfuna PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
                 $<INSTALL_INTERFACE:include>)

install(
  TARGETS ${PROJECT_NAME}
  DESTINATION ${LIBRARY_INSTALL_DIR}
  EXPORT LibFunATargets)
install(EXPORT LibFunATargets DESTINATION ${LIBRARY_INSTALL_DIR}/cmake/LibFunA)
install(DIRECTORY "include/" DESTINATION ${INCLUDE_INSTALL_DIR}/)
