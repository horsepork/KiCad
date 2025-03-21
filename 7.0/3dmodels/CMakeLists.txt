cmake_minimum_required( VERSION 3.10 FATAL_ERROR )

project( kicad-packages3d NONE )


#================================================
# Locations for install targets.
#================================================
if( APPLE )
    # Like all variables, CMAKE_INSTALL_PREFIX can be over-ridden on the command line.
    set( CMAKE_INSTALL_PREFIX "/Library/Application Support/kicad/" CACHE PATH "" )
    # Everything without leading / is relative to CMAKE_INSTALL_PREFIX.
    set( KICAD_PACKAGES_3D 3dmodels )
else()
    # Everything without leading / is relative to CMAKE_INSTALL_PREFIX.
    set( KICAD_DATA share/kicad
        CACHE PATH "Location of KiCad data files." )
    set( KICAD_PACKAGES_3D ${KICAD_DATA}/3dmodels )
endif()

mark_as_advanced( KICAD_DATA KICAD_MODULES )


#================================================
# "make uninstall" rules
#
# The uninstaller does not remove folders only files.
#================================================
configure_file(
    "${PROJECT_SOURCE_DIR}/CMakeModules/cmake_uninstall.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
    IMMEDIATE @ONLY
)

add_custom_target( uninstall
    "${CMAKE_COMMAND}" -P "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
)


#================================================
# Installed files.
#================================================

# Install all of the .3dshapes folders and their contents in to the modules path.
file ( GLOB SHAPE_DIRS "*.3dshapes" )

install( DIRECTORY ${SHAPE_DIRS}
    DESTINATION ${KICAD_PACKAGES_3D}
    COMPONENT resources
    FILES_MATCHING PATTERN "*.wrl"
    PATTERN ".git" EXCLUDE
)

install( DIRECTORY ${SHAPE_DIRS}
    DESTINATION ${KICAD_PACKAGES_3D}
    COMPONENT resources
    FILES_MATCHING PATTERN "*.step"
    PATTERN ".git" EXCLUDE
)
