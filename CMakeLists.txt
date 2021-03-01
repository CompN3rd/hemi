# largely copied from: https://dominikberner.ch/cmake-interface-lib/

cmake_minimum_required(VERSION 3.12)
project("hemi" VERSION 2.0
               DESCRIPTION "Hemi simplifies writing portable CUDA C/C++ code."
               HOMEPAGE_URL "http://harrism.github.io/hemi/")

include(GNUInstallDirs)

add_library(${PROJECT_NAME} INTERFACE)
target_include_directories(${PROJECT_NAME} INTERFACE $<BUILD_INTERFACE:${${PROJECT_NAME}_SOURCE_DIR}>
                                                     $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)
target_compile_features(${PROJECT_NAME} INTERFACE cxx_std_11)

install(TARGETS ${PROJECT_NAME}
        EXPORT ${PROJECT_NAME}_Targets
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})

include(CMakePackageConfigHelpers)
write_basic_package_version_file("${PROJECT_NAME}ConfigVersion.cmake"
                                 VERSION ${PROJECT_VERSION}
                                 COMPATIBILITY SameMajorVersion)

configure_package_config_file("${PROJECT_SOURCE_DIR}/cmake/${PROJECT_NAME}Config.cmake.in"
                              "${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
                              INSTALL_DESTINATION
                              ${CMAKE_INSTALL_DATAROOTDIR}/${PROJECT_NAME}/cmake)
                            
install(EXPORT ${PROJECT_NAME}_Targets
        FILE ${PROJECT_NAME}Targets.cmake
        NAMESPACE ${PROJECT_NAME}::
        DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/${PROJECT_NAME}/cmake)

install(FILES "${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
              "${PROJECT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
        DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/${PROJECT_NAME}/cmake)

install(DIRECTORY ${PROJECT_SOURCE_DIR}/hemi DESTINATION include)