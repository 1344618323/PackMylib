cmake_minimum_required(VERSION 3.10)
project(libfunb)
add_library(libfunb SHARED ${CMAKE_CURRENT_SOURCE_DIR}/src/libb.cpp)
target_include_directories(
  libfunb PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
                 $<INSTALL_INTERFACE:include>)

install(
  TARGETS ${PROJECT_NAME}
  DESTINATION ${LIBRARY_INSTALL_DIR}
  EXPORT LibFunBTargets)
install(EXPORT LibFunBTargets DESTINATION ${LIBRARY_INSTALL_DIR}/cmake/LibFunB)
install(DIRECTORY "include/" DESTINATION ${INCLUDE_INSTALL_DIR}/)
