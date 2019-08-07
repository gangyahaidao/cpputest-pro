#要求最低的cmake版本
cmkae_minimum_required(VERSION 3.5)

#禁止编译时污染源码树
set(CMAKE_DISABLE_IN_SOURCE_BUILD on)
set(CMAKE_DISABLE_SOURCE_CHANGES on)

enable_language(C)
enable_language(CXX)

#添加c++11标准支持
#set( CMAKE_CXX_FLAGS "-std=c++11" )

set(CFLAGS "-Wall -Werror")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${CFLAGS}")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${CFLAGS}")
enable_testing()

#加载子目录中的cmake文件
add_subdirectory(src)
add_subdirectory(test)