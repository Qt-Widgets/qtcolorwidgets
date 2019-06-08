cmake_minimum_required(VERSION 3.0)
include(GNUInstallDirs)

project(QtColorWidgets VERSION 1.0.0)
set(CMAKE_AUTOMOC ON)
include_directories(${CMAKE_BINARY_DIR})

if(CMAKE_BUILD_TYPE MATCHES "^(release|Release|RELEASE)$")
    add_definitions(-DQT_NO_DEBUG_OUTPUT)
else()
    message("-- DEBUG BUILD")
endif()
add_compile_options(-Wall -Wextra)

option(USE_QT5 "Use Qt 5" OFF)
if(USE_QT5)
    find_package(Qt5Core REQUIRED)
    find_package(Qt5Gui REQUIRED)
    find_package(Qt5Widgets REQUIRED)
else()
    find_package(Qt4 REQUIRED QtCore QtGui)
endif()

# library
add_library(
    ${PROJECT_NAME}
    SHARED
    src/qtcolorpicker.cpp
    src/qtcolortriangle.cpp
    # TODO missing widgets
)
set_target_properties(
    ${PROJECT_NAME}
    PROPERTIES
    VERSION
    ${PROJECT_VERSION}
)
set_target_properties(
    ${PROJECT_NAME}
    PROPERTIES
    SOVERSION
    ${PROJECT_VERSION_MAJOR}
)
target_include_directories(${PROJECT_NAME} PUBLIC src)

# link
if(USE_QT5)
    target_link_libraries(${PROJECT_NAME} Qt5::Core Qt5::Gui Qt5::Widgets)
else()
    target_link_libraries(${PROJECT_NAME} Qt4::QtCore Qt4::QtGui)
endif()

# install
# TODO add pkgconfig stuff and headers
if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    set(LIB_INSTALL_DIR ${CMAKE_INSTALL_LIBDIR})
else()
    set(LIB_INSTALL_DIR ${CMAKE_INSTALL_FULL_LIBDIR})
endif()
install(
    TARGETS
    ${PROJECT_NAME}
    DESTINATION
    ${LIB_INSTALL_DIR}
)
install(FILES
        docs/LGPL_EXCEPTION.txt
        docs/README.md
        LICENSE
        DESTINATION
        ${CMAKE_INSTALL_DOCDIR}/${PROJECT_NAME}-${PROJECT_VERSION}${PROJECT_VERSION_EXTRA})
