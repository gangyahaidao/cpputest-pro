cmake_minimum_required(VERSION 3.5)

#设置工程的名字和版本
project(collectc VERSION 0.0.1)

#创建一个变量用来保存正则表达式匹配的文件
file(GLOB source_files "*.c")
file(GLOB header_files "include/*.h")

#创建动态和静态共享库，命名lib${PROJECT_NAME}.so或者.a后缀
add_library(${PROJECT_NAME} SHARED ${source_files})
add_library(${PROJECT_NAME}_static STATIC ${source_files})
#设置包含的头文件
inlcude_directories("./include")

set_target_properties(${PROJECT_NAME} PROPERTIES PUBLIC_HEADER "${header_files}")
set_target_properties(${PROJECT_NAME}_static PROPERTIES OUTPUT_NAME ${PROJECT_NAME})

#添加可执行文件需要的库
target_link_libraries(${PROJECT_NAME})

set(${PROJECT_NAME}_INCLUDE_DIRS ${PROJECT_SOURCE_DIR}/include
  CACHE INTERNAL "${PROJECT_NAME}: Include directories" FORCE)

#加载CMake的pkg-config模块，REQUIRED、QUIET是否打印状态信息
include(FindPkgConfig QUIET)
if(PKG_CONFIG_FOUND)
  #将collectionc.pc.in文件复制为collectionc.pc，同时替换掉源文件中的@VAR@变量的值
  configure_file("collectionc.pc.in" "collectionc.pc" @ONLY)
  install(FILES "${CMAKE_CURRENT_BINARY_DIR}/collectionc.pc"
    DESTINATION "${CMAKE_INSTALL_PREFIX}/lib/pkgconfig")
endif()

#执行make install命令将生成的库文件安装到系统路径中
install(TARGETS ${PROJECT_NAME} ${PROJECT_NAME}_static
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  PUBLIC_HEADER DESTINATION include/${PROJECT_NAME}
  )