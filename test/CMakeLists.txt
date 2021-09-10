cmake_minimum_required(VERSION 3.10)
project(testmain)
list(APPEND CMAKE_PREFIX_PATH "/home/cxn/code/cplus/package")
find_package(MyLib)
if(MyLib_FOUND)
  message("LibFun found")
  message(${MyLib_INCLUDE_DIRS})
  message(${MyLib_LIBRARY_DIRS})
endif()

# 注意！不要忘了添加链接库的地址
link_directories(${MyLib_LIBRARY_DIRS})

add_executable(testmain main.cpp)
target_include_directories(testmain PUBLIC ${CMAKE_CURRENT_SOURCE_DIR} ${MyLib_INCLUDE_DIRS})


target_link_libraries(testmain PUBLIC ${MyLib_LIBRARIES})
# 这一句顶两句
#target_link_libraries(testmain PUBLIC libfuna)
#target_link_libraries(testmain PUBLIC libfunb)