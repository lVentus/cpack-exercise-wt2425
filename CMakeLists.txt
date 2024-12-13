cmake_minimum_required(VERSION "3.28")

project("cpackexample" VERSION 0.1.0)

FIND_PACKAGE(deal.II 9.5 REQUIRED
  HINTS ${DEAL_II_DIR} ../ ../../ $ENV{DEAL_II_DIR}
)
DEAL_II_INITIALIZE_CACHED_VARIABLES()

find_package(Boost 1.83 COMPONENTS filesystem REQUIRED)
find_package(yaml-cpp 0.6 REQUIRED)

add_library(cpackexamplelib 
    filesystem/filesystem.cpp 
    fem/fem.cpp 
    flatset/flatset.cpp 
    yamlParser/yamlParser.cpp
)

add_executable("${PROJECT_NAME}" main.cpp)

target_link_libraries("${PROJECT_NAME}" cpackexamplelib)
target_link_libraries(cpackexamplelib Boost::filesystem ${YAML_CPP_LIBRARIES})

DEAL_II_SETUP_TARGET("${PROJECT_NAME}")
DEAL_II_SETUP_TARGET(cpackexamplelib)

include(GNUInstallDirs)

install(TARGETS cpackexamplelib
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/cpackexamplelib)

install(TARGETS "${PROJECT_NAME}"
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})

install(DIRECTORY fem/ filesystem/ flatset/ yamlParser/
        DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/cpackexamplelib
        FILES_MATCHING PATTERN "*.hpp")

include(cmake/CPackConfig.cmake)
