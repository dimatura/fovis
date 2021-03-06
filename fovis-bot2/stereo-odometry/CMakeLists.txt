pkg_check_modules(BOT_CORE bot2-core)
if(NOT BOT_CORE_FOUND)
    message("bot2-core not found.  Not building stereo-odometry")
    return()
endif()

add_executable(stereo-odometry
    jpeg-utils.c
    main.cpp
    stereo-odometry.cpp
    visualization.cpp
    )

pods_use_pkg_config_packages(stereo-odometry
    glib-2.0
    lcm
    libfovis
    bot2-core
    bot2-param-client
    )

# link against the LCM types defined in ../lcmtypes/
target_link_libraries(stereo-odometry ${LCMTYPES_LIBS})

# use LCMGL if available
pkg_check_modules(LCMGL bot2-lcmgl-client)
if(LCMGL_FOUND)
    target_link_libraries(stereo-odometry ${LCMGL_LIBRARIES})
    include_directories(${LCMGL_INCLUDE_DIRS})
    add_definitions(-DUSE_LCMGL)
endif()

pods_install_executables(stereo-odometry)

target_link_libraries(stereo-odometry jpeg)
