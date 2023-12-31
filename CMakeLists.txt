cmake_minimum_required(VERSION 3.5)

project(App VERSION 0.1 LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(QT NAMES Qt6 Qt5 COMPONENTS Widgets REQUIRED)
find_package(Qt${QT_VERSION_MAJOR} COMPONENTS Widgets REQUIRED)
find_package(Qt5 REQUIRED COMPONENTS Network)

set(PROJECT_SOURCES
        main.cpp
        mainwindow.cpp
        mainwindow.h
        mainwindow.ui

        videocamfeedwidget.h
        videocamfeedwidget.cpp
        videocamfeedwidget.ui

        exercisewindowwidget.h
        exercisewindowwidget.cpp
        exercisewindowwidget.ui

        controlwindowwidget.h
        controlwindowwidget.cpp
        controlwindowwidget.ui

        availableexercisewidget.h
        availableexercisewidget.cpp
        availableexercisewidget.ui

        exerciseobject.h
        exerciseobject.cpp
        exerciseobject.ui

        clientinfowidget.h
        clientinfowidget.cpp
        clientinfowidget.ui

        dialogwindowwidget.h
        dialogwindowwidget.cpp
        dialogwindowwidget.ui

        MyClasses.cpp
        MyClasses.h
        mjpegstreamer.cpp
        mjpegstreamer.h
        resources/resources.qrc
)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(App
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
    )
# Define target properties for Android with Qt 6 as:
#    set_property(TARGET App APPEND PROPERTY QT_ANDROID_PACKAGE_SOURCE_DIR
#                 ${CMAKE_CURRENT_SOURCE_DIR}/android)
# For more information, see https://doc.qt.io/qt-6/qt-add-executable.html#target-creation
else()
    if(ANDROID)
        add_library(App SHARED
            ${PROJECT_SOURCES}
        )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
    else()
        add_executable(App
            ${PROJECT_SOURCES}
        )
    endif()
endif()

target_link_libraries(App PRIVATE Qt${QT_VERSION_MAJOR}::Widgets Qt5::Network)

set_target_properties(App PROPERTIES
    MACOSX_BUNDLE_GUI_IDENTIFIER my.example.com
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
)

if(QT_VERSION_MAJOR EQUAL 6)
    qt_finalize_executable(App)
endif()
