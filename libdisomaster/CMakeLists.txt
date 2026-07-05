cmake_minimum_required(VERSION 3.16)
project(disomaster-qt6_lib LANGUAGES CXX)

find_package(Qt6 COMPONENTS Core REQUIRED)
find_package(PkgConfig REQUIRED)
pkg_check_modules(LIBISOBURN REQUIRED libisoburn-1)

set(CMAKE_AUTOMOC ON)

set(SOURCES disomaster.cpp)
set(HEADERS disomaster.h)

# Headers to disomaster-qt6/ so that won't conflict w/ Qt5 ver.
add_library(disomaster6 SHARED ${SOURCES} ${HEADERS})

set_target_properties(disomaster6 PROPERTIES
    VERSION 1.0.0
    SOVERSION 1
)

target_compile_definitions(disomaster6 PRIVATE
    DISOMASTER_LIBRARY
    QT_DEPRECATED_WARNINGS
)

target_link_libraries(disomaster6
    PUBLIC Qt6::Core
    PRIVATE ${LIBISOBURN_LIBRARIES}
)

target_include_directories(disomaster6 PRIVATE ${LIBISOBURN_INCLUDE_DIRS})

# ----- 安装库和头文件（供 debian 打包）-----
install(TARGETS disomaster6
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

install(FILES ${HEADERS}
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/disomaster-qt6
)

# ----- 生成并安装 disomaster.pc -----
set(prefix      ${CMAKE_INSTALL_PREFIX})
set(exec_prefix ${prefix})
set(libdir      ${prefix}/${CMAKE_INSTALL_LIBDIR})
set(includedir  ${prefix}/${CMAKE_INSTALL_INCLUDEDIR}/disomaster-qt6)

# 构造 .pc 内容（文件名与 .install 中一致：disomaster.pc）
set(PC_CONTENT "
prefix=${prefix}
exec_prefix=${exec_prefix}
libdir=${libdir}
includedir=${includedir}

Name: libdisomaster-qt6
Description: Wrapper class for libisoburn
Version: ${PROJECT_VERSION}
Requires: libisoburn-1
Libs: -L\${libdir} -ldisomaster6
Cflags: -I\${includedir}
")

# 生成文件（直接写入构建目录）
file(GENERATE OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/disomaster-qt6.pc CONTENT "${PC_CONTENT}")

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/disomaster-qt6.pc
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig
)