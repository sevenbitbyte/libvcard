cmake_minimum_required (VERSION 2.8.8)
project (vcard)

if(CMAKE_BUILD_TYPE STREQUAL Release)
  SET(CMAKE_BUILD_TYPE Release)
  message("Release mode")
else()
  SET(CMAKE_BUILD_TYPE Debug)
  SET(CMAKE_CXX_FILES "-g -Wall")
  message("Debug mode")
endif()

#find QT libraries
find_package(Qt5Core REQUIRED)

# Instruct CMake to run moc automatically when needed.
set(CMAKE_AUTOMOC ON)

# As moc files are generated in the binary dir, tell CMake
# to always look for includes there:
set(CMAKE_INCLUDE_CURRENT_DIR ON)
INCLUDE_DIRECTORIES( ${CMAKE_CURRENT_BINARY_DIR} )

set(VCARD_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/libvcard)
set(VCARD_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include/vcard)

#Configure output directories
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}/lib)


include_directories(BEFORE
    ${VCARD_INCLUDE_DIR}
    ${VCARD_SOURCE_DIR}
    ${Qt5Core_INCLUDE_DIRS}
    ${Qt5Network_INCLUDE_DIRS}
    ${Qt5Sql_INCLUDE_DIRS}
)


set(VCARD_LIB_SOURCE
    ${VCARD_SOURCE_DIR}/vcard.cpp
    ${VCARD_SOURCE_DIR}/vcardparam.cpp
    ${VCARD_SOURCE_DIR}/vcardproperty.cpp
)


set(VCARD_LIB_HEADERS
    ${VCARD_INCLUDE_DIR}/libvcard_global.h
    ${VCARD_INCLUDE_DIR}/vcard.h
    ${VCARD_INCLUDE_DIR}/vcardparam.h
    ${VCARD_INCLUDE_DIR}/vcardproperty.h
)

add_library(vcard ${VCARD_LIB_SOURCE} ${VCARD_LIB_HEADERS})
qt5_use_modules(vcard Core)

INSTALL(DIRECTORY ${VCARD_INCLUDE_DIR} DESTINATION include)

INSTALL(TARGETS vcard
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
)
