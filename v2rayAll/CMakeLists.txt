cmake_minimum_required(VERSION 3.5)

project(v2rayAll LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# QtCreator supports the following variables for Android, which are identical to qmake Android variables.
# Check http://doc.qt.io/qt-5/deployment-android.html for more information.
# They need to be set before the find_package(Qt5 ...) call.

#if(ANDROID)
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
#    if (ANDROID_ABI STREQUAL "armeabi-v7a")
#        set(ANDROID_EXTRA_LIBS
#            ${CMAKE_CURRENT_SOURCE_DIR}/path/to/libcrypto.so
#            ${CMAKE_CURRENT_SOURCE_DIR}/path/to/libssl.so)
#    endif()
#endif()

find_package(Qt5 COMPONENTS Core Quick LinguistTools REQUIRED)

set(TS_FILES v2rayAll_zh_CN.ts)

if(ANDROID)
    add_library(v2rayAll SHARED
      main.cpp
      qml.qrc
      ${TS_FILES}
    )
else()
    add_executable(v2rayAll
      main.cpp
      qml.qrc
      icon.rc
      ${TS_FILES}
    )
endif()

target_compile_definitions(v2rayAll
  PRIVATE $<$<OR:$<CONFIG:Debug>,$<CONFIG:RelWithDebInfo>>:QT_QML_DEBUG>)
target_link_libraries(v2rayAll
  PRIVATE Qt5::Core Qt5::Quick)

qt5_create_translation(QM_FILES ${CMAKE_SOURCE_DIR} ${TS_FILES})