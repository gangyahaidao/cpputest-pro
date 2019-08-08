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
include_directories("./include")

set_target_properties(${PROJECT_NAME} PROPERTIES PUBLIC_HEADER "${header_files}")
set_target_properties(${PROJECT_NAME}_static PROPERTIES OUTPUT_NAME ${PROJECT_NAME})

#添加可执行文件需要的库
target_link_libraries(${PROJECT_NAME})

set(${PROJECT_NAME}_INCLUDE_DIRS ${PROJECT_SOURCE_DIR}/include
  CACHE INTERNAL "${PROJECT_NAME}: Include directories" FORCE)

#生成pkg-config的支持文件，REQUIRED、QUIET是否打印状态信息
include(FindPkgConfig QUIET)
if(PKG_CONFIG_FOUND)
  #将collectionc.pc.in文件复制为collectionc.pc，同时替换掉源文件中的@VAR@变量的值，之后在在使用这个库文件的时候就可以使用pkg-config进行自动配置了
  #如：gcc file.c -o file `pkg-config --cflags --libs collectionc` 会根据.pc文件中的配置自动找到lib和include头文件
  configure_file("collectionc.pc.in" "libcollectionc.pc" @ONLY)
  install(FILES "${CMAKE_CURRENT_BINARY_DIR}/libcollectionc.pc"
    DESTINATION "${CMAKE_INSTALL_PREFIX}/lib/pkgconfig")
endif()

#执行make install命令将生成的库文件安装到系统路径中
install(TARGETS ${PROJECT_NAME} ${PROJECT_NAME}_static
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  PUBLIC_HEADER DESTINATION include/${PROJECT_NAME}
  )