cmake_minimum_required(VERSION 3.5)
project(collectc_test)

find_package(PkgConfig)
# 加载依赖的测试框架库和头文件
pkg_check_modules(CPPUTEST REQUIRED cpputest>=3.8)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${CPPUTEST_CXX_FLAGS}")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${CPPUTEST_C_FLAGS}")

set(list_test_sources list_test.c listTest.cpp)

#设置包含的文件路径
#message(WARNING ${PROJECT_SOURCE_DIR}/include) 用于包含本工程中的头文件路径
#message(WARNING ${collectc_INCLUDE_DIRS}) #用于包含collectc工程中的头文件路径
include_directories(${PROJECT_SOURCE_DIR}/include ${collectc_INCLUDE_DIRS} ${CPPUTEST_INCLUDE_DIRS})

#设置可执行文件名
add_executable(list_test ${list_test_sources})

#添加可执行文件所需的库
target_link_libraries(list_test collectc ${CPPUTEST_LDFLAGS})

add_test(ListTest list_test -c -v)