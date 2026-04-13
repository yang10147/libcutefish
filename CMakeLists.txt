cmake_minimum_required(VERSION 3.16)
project(libcutefish)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(GenerateExportHeader)
include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

set(QT Core Gui Quick QuickControls2 Widgets DBus Xml Concurrent)
find_package(Qt6 REQUIRED ${QT})

# Get QML install directory from qmake6
find_program(QT_QMAKE_EXECUTABLE NAMES qmake6 qmake)
if(NOT QT_QMAKE_EXECUTABLE)
    message(FATAL_ERROR "qmake not found.")
endif()

execute_process(COMMAND ${QT_QMAKE_EXECUTABLE} -query QT_INSTALL_QML
    OUTPUT_VARIABLE INSTALL_QMLDIR
    OUTPUT_STRIP_TRAILING_WHITESPACE
)
if(INSTALL_QMLDIR)
    message(STATUS "qml directory: ${INSTALL_QMLDIR}")
else()
    message(FATAL_ERROR "qml directory cannot be detected.")
endif()

add_subdirectory(accounts)
add_subdirectory(system)
# 以下待移植完成后逐步启用：
# add_subdirectory(bluez)
add_subdirectory(networkmanagement)
# add_subdirectory(screen)
# add_subdirectory(audio)
# add_subdirectory(mpris)
