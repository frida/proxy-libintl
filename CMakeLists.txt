cmake_minimum_required(VERSION 3.24)

project(proxy-libintl VERSION 1 LANGUAGES C)

include(GNUInstallDirs)

option(BUILD_SHARED_LIBS "Build shared libraries by default" ON)

add_library(intl libintl.c)

target_compile_features(intl PRIVATE c_std_99)
target_compile_definitions(intl PRIVATE STUB_ONLY=1)

if(MSVC)
  target_compile_options(intl PRIVATE /W1)
endif()

if(NOT BUILD_SHARED_LIBS)
  target_compile_definitions(intl PUBLIC G_INTL_STATIC_COMPILATION)
endif()

set_target_properties(intl PROPERTIES
  SOVERSION 8
  VERSION 8.0.0
)

target_include_directories(intl
  PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

install(FILES libintl.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(
  TARGETS intl
  EXPORT proxy-libintlTargets
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

install(
  EXPORT proxy-libintlTargets
  NAMESPACE proxy_libintl::
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/proxy-libintl
)
