cmake_minimum_required(VERSION 3.14)

project(TPClientQt
  DESCRIPTION "Network client for Touch Portal plugin API."
  LANGUAGES CXX
)

option(BUILD_SHARED_LIBS "Build project as a shared (DLL) library when ON, or as static lib when OFF." TRUE)

set(CMAKE_AUTOMOC ON)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)




find_package(QT NAMES Qt5 Qt6 REQUIRED)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS
        Core
        Network
)
add_library(${PROJECT_NAME} TPClientQt.h TPClientQt.cpp)
target_link_libraries(${PROJECT_NAME} PRIVATE
        Qt${QT_VERSION_MAJOR}::Core
        Qt${QT_VERSION_MAJOR}::Network
)

if (BUILD_SHARED_LIBS)
  target_compile_definitions(${PROJECT_NAME} PRIVATE TPCLIENT_BUILD_DLL)
endif()

target_compile_definitions(${PROJECT_NAME} PRIVATE
        QT_USE_QSTRINGBUILDER
        QT_MESSAGELOGCONTEXT
        QT_NO_KEYWORDS
)
message(WARNING ${CMAKE_CURRENT_LIST_DIR})

set_target_properties(${PROJECT_NAME} PROPERTIES PUBLIC_HEADER TPClientQt.h)
install(TARGETS ${PROJECT_NAME} LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}" PUBLIC_HEADER DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}")
install(DIRECTORY ${CMAKE_CURRENT_LIST_DIR}
        DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}"
        FILES_MATCHING PATTERN "*.h"
)