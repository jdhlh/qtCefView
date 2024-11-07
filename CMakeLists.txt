CMAKE_MINIMUM_REQUIRED(VERSION 3.5)

PROJECT(qtcefdemo)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 【QT】CMAKE_PREFIX_PATH 实际值为本地安装的QT中的对应编译环境的目录
# SET(CMAKE_PREFIX_PATH "D:\\Programs\\Qt\\Qt5.14.2\\5.14.2\\msvc2017_64")
# 配置了上述后，可以通过find_package来查找QT相关的cmake文件

# 【QT】UIC、MOC、RCC启用
# 引入的QT模块则会对.ui文件、.qtc文件以及QT中的元信息机制自动进行处理
SET(CMAKE_AUTOUIC ON)
SET(CMAKE_AUTOMOC ON)
SET(CMAKE_AUTORCC ON)

# 【QT】通过FIND_PACKAGE，CMake会查找QT相关模块cmake文件，
# 这些cmake文件自动处理了头文件的查找等，
# 不需要像配置CEF的头文件查找一样来配置QT的头文件引入
find_package(Qt5 COMPONENTS Widgets Gui REQUIRED)
find_package(X11 REQUIRED)
# 【CEF】CEF相关头文件的引入
INCLUDE_DIRECTORIES("${CMAKE_SOURCE_DIR}/CefFiles")


# 添加项目所有的文件：
# 头文件、源文件、ui文件、qrc资源文件
set(CEFSIMPLE_SRCS
    cefsimple_linux.cpp
    simple_app.cpp
    simple_app.h
    simple_handler.cpp
    simple_handler.h
    simple_handler_linux.cpp
    )

add_executable(qtcefdemo ${CEFSIMPLE_SRCS})
#if(CMAKE_BUILD_TYPE STREQUAL "Debug")
#message("Building in Debug mode")
set(CEF_LIB "${CMAKE_SOURCE_DIR}/CefFiles/bin/Debug/libcef.so")
set(CEF_DLL_WRAPPER "${CMAKE_SOURCE_DIR}/CefFiles/bin/Debug/libcef_dll_wrapper.a")
#else()
#message("Building in Release mode")
#set(CEF_LIB "${CMAKE_SOURCE_DIR}/CefFiles/bin/Release/libcef.so")
#set(CEF_DLL_WRAPPER "${CMAKE_SOURCE_DIR}/CefFiles/bin/Release/libcef_dll_wrapper.a")
#endif()
# QT库链接
target_link_libraries(
    qtcefdemo
    PRIVATE
    pthread
    # 【QT】QT库链接
    Qt5::Widgets
    Qt5::Gui
    # 【CEF】cef相关库链接
    ${CEF_LIB}
    ${CEF_DLL_WRAPPER}
    ${X11_LIBRARIES}
    )
