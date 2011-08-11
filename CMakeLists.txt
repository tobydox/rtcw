CMAKE_MINIMUM_REQUIRED(VERSION 2.6.0)

PROJECT(RtCW-SP)

SET(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake ${CMAKE_MODULE_PATH})

SET(VERSION_MAJOR "1")
SET(VERSION_MINOR "41")
SET(VERSION_PATCH "0")
#SET(VERSION_SUFFIX "")

SET(VERSION "${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH}")
IF(VERSION_SUFFIX)
	SET(VERSION "${VERSION}-${VERSION_SUFFIX}")
ENDIF(VERSION_SUFFIX)

IF(WIN32)
	SET(SDL_INCLUDE_DIR ${MINGW_PREFIX}/include/SDL)
	SET(SDL_LIBRARY -lSDL -lSDLmain)
ELSE()
	FIND_PACKAGE(SDL REQUIRED)
ENDIF()

FIND_PACKAGE(ZLIB REQUIRED)
FIND_PACKAGE(OpenGL REQUIRED)

MESSAGE("PROCESSOR: ${CMAKE_SYSTEM_PROCESSOR}")

IF(WIN32)
    IF(WIN64)
        SET(IS_X86_64 TRUE)
    ELSE(WIN64)
        SET(IS_X86 TRUE)
    ENDIF(WIN64)
ELSE(WIN32)
    EXEC_PROGRAM( ${CMAKE_C_COMPILER} ARGS "-dumpmachine" OUTPUT_VARIABLE Machine )
    MESSAGE("Machine: ${Machine}")
    STRING(REGEX MATCH "i.86" IS_X86 "${Machine}")
    STRING(REGEX MATCH "86_64" IS_X86_64 "${Machine}")
ENDIF(WIN32)

# set compiler flags
SET(CMAKE_C_FLAGS "-Wall -O2 -fno-strict-aliasing -Wno-unused-but-set-variable ${CMAKE_C_FLAGS}")
SET(CMAKE_CXX_FLAGS "-Wall -O2 -Wno-unused-but-set-variable -Wno-write-strings ${CMAKE_CXX_FLAGS}")

ADD_DEFINITIONS(-DBOTLIB -DC_ONLY)

SET(S ${CMAKE_SOURCE_DIR}/src)

SET(rtcw_SOURCES
	${S}/client/cl_cgame.c
	${S}/client/cl_cin.c
	${S}/client/cl_console.c
	${S}/client/cl_input.c
	${S}/client/cl_keys.c
	${S}/client/cl_main.c
	${S}/client/cl_net_chan.c
	${S}/client/cl_parse.c
	${S}/client/cl_scrn.c
	${S}/client/cl_ui.c
	${S}/client/snd_adpcm.c
	${S}/client/snd_dma.c
	${S}/client/snd_mem.c
	${S}/client/snd_mix.c
	${S}/client/snd_wavelet.c

	${S}/qcommon/cm_load.c
	${S}/qcommon/cm_patch.c
	${S}/qcommon/cm_polylib.c
	${S}/qcommon/cm_test.c
	${S}/qcommon/cm_trace.c
	${S}/qcommon/cmd.c
	${S}/qcommon/common.c
	${S}/qcommon/cvar.c
	${S}/qcommon/files.c
	${S}/qcommon/md4.c
	${S}/qcommon/msg.c
	${S}/qcommon/net_chan.c
	${S}/qcommon/net_ip.c
	${S}/qcommon/huffman.c
	${S}/qcommon/unzip.c
	${S}/qcommon/vm.c

	${S}/server/sv_bot.c
	${S}/server/sv_ccmds.c
	${S}/server/sv_client.c
	${S}/server/sv_game.c
	${S}/server/sv_init.c
	${S}/server/sv_main.c
	${S}/server/sv_net_chan.c
	${S}/server/sv_snapshot.c
	${S}/server/sv_world.c

	${S}/game/q_math.c
	${S}/game/q_shared.c

	${S}/botlib/be_aas_bspq3.c
	${S}/botlib/be_aas_cluster.c
	${S}/botlib/be_aas_debug.c
	${S}/botlib/be_aas_entity.c
	${S}/botlib/be_aas_file.c
	${S}/botlib/be_aas_main.c
	${S}/botlib/be_aas_move.c
	${S}/botlib/be_aas_optimize.c
	${S}/botlib/be_aas_reach.c
	${S}/botlib/be_aas_route.c
	${S}/botlib/be_aas_routealt.c
	${S}/botlib/be_aas_routetable.c
	${S}/botlib/be_aas_sample.c
	${S}/botlib/be_ai_char.c
	${S}/botlib/be_ai_chat.c
	${S}/botlib/be_ai_gen.c
	${S}/botlib/be_ai_goal.c
	${S}/botlib/be_ai_move.c
	${S}/botlib/be_ai_weap.c
	${S}/botlib/be_ai_weight.c
	${S}/botlib/be_ea.c
	${S}/botlib/be_interface.c
	${S}/botlib/l_crc.c
	${S}/botlib/l_libvar.c
	${S}/botlib/l_log.c
	${S}/botlib/l_memory.c
	${S}/botlib/l_precomp.c
	${S}/botlib/l_script.c
	${S}/botlib/l_struct.c

	${S}/jpeg-6/jcapimin.c
	${S}/jpeg-6/jccoefct.c
	${S}/jpeg-6/jccolor.c
	${S}/jpeg-6/jcdctmgr.c
	${S}/jpeg-6/jchuff.c
	${S}/jpeg-6/jcinit.c
	${S}/jpeg-6/jcmainct.c
	${S}/jpeg-6/jcmarker.c
	${S}/jpeg-6/jcmaster.c
	${S}/jpeg-6/jcomapi.c
	${S}/jpeg-6/jcparam.c
	${S}/jpeg-6/jcphuff.c
	${S}/jpeg-6/jcprepct.c
	${S}/jpeg-6/jcsample.c
	${S}/jpeg-6/jdapimin.c
	${S}/jpeg-6/jdapistd.c
	${S}/jpeg-6/jdatasrc.c
	${S}/jpeg-6/jdcoefct.c
	${S}/jpeg-6/jdcolor.c
	${S}/jpeg-6/jddctmgr.c
	${S}/jpeg-6/jdhuff.c
	${S}/jpeg-6/jdinput.c
	${S}/jpeg-6/jdmainct.c
	${S}/jpeg-6/jdmarker.c
	${S}/jpeg-6/jdmaster.c
	${S}/jpeg-6/jdpostct.c
	${S}/jpeg-6/jdsample.c
	${S}/jpeg-6/jdtrans.c
	${S}/jpeg-6/jerror.c
	${S}/jpeg-6/jfdctflt.c
	${S}/jpeg-6/jidctflt.c
	${S}/jpeg-6/jmemmgr.c
	${S}/jpeg-6/jmemnobs.c
	${S}/jpeg-6/jutils.c

	${S}/renderer/tr_animation.c
	${S}/renderer/tr_backend.c
	${S}/renderer/tr_bsp.c
	${S}/renderer/tr_cmds.c
	${S}/renderer/tr_cmesh.c
	${S}/renderer/tr_curve.c
	${S}/renderer/tr_flares.c
	${S}/renderer/tr_font.c
	${S}/renderer/tr_image.c
	${S}/renderer/tr_init.c
	${S}/renderer/tr_light.c
	${S}/renderer/tr_main.c
	${S}/renderer/tr_marks.c
	${S}/renderer/tr_mesh.c
	${S}/renderer/tr_model.c
	${S}/renderer/tr_noise.c
	${S}/renderer/tr_scene.c
	${S}/renderer/tr_shade.c
	${S}/renderer/tr_shade_calc.c
	${S}/renderer/tr_shader.c
	${S}/renderer/tr_shadows.c
	${S}/renderer/tr_sky.c
	${S}/renderer/tr_surface.c
	${S}/renderer/tr_world.c

	${S}/splines/math_angles.cpp
	${S}/splines/math_matrix.cpp
	${S}/splines/math_quaternion.cpp
	${S}/splines/math_vector.cpp
	${S}/splines/q_parse.cpp
	${S}/splines/splines.cpp
	${S}/splines/util_str.cpp

	${S}/sdl/sdl_gamma.c
	${S}/sdl/sdl_input.c
	${S}/sdl/sdl_snd.c
	${S}/sdl/sdl_glimp.c

	${S}/sys/con_passive.c
	${S}/sys/con_log.c
	${S}/sys/sys_main.c
	)

IF(WIN32)
	SET(rtcw_SOURCES ${rtcw_SOURCES} ${S}/sys/sys_win32.c)
	ADD_DEFINITIONS(-DUSE_ICON)
ENDIF()

IF(UNIX)
	SET(rtcw_SOURCES ${rtcw_SOURCES} ${S}/sys/sys_unix.c)
ENDIF()

IF(WIN32)
	LINK_LIBRARIES(-lws2_32 -lwinmm -lpsapi -lgdi32 -lole32)
	INSTALL(FILES
				${QT_BINARY_DIR}/SDL.dll
				${QT_BINARY_DIR}/zlib1.dll
				DESTINATION .)
ENDIF(WIN32)

INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR} ${ZLIB_INCLUDE_DIR} ${SDL_INCLUDE_DIR})
LINK_LIBRARIES(-lm ${ZLIB_LIBRARIES} ${SDL_LIBRARY} ${OPENGL_gl_LIBRARY})
ADD_EXECUTABLE(rtcw-sp ${rtcw_SOURCES})

IF(WIN32)
	SET_TARGET_PROPERTIES(rtcw-sp PROPERTIES LINK_FLAGS "${LINK_FLAGS} -mwindows")
	ADD_CUSTOM_COMMAND(TARGET rtcw-sp POST_BUILD COMMAND ${STRIP} ${CMAKE_BINARY_DIR}/rtcw-sp.exe)
	INSTALL(TARGETS rtcw-sp RUNTIME DESTINATION .)
ELSE(WIN32)
	INSTALL(TARGETS rtcw-sp RUNTIME DESTINATION bin)
ENDIF(WIN32)

ADD_SUBDIRECTORY(main)

#
# add dist target
#
SET(TMP "rtcw-sp-${VERSION}")
SET(S ${CMAKE_SOURCE_DIR})
ADD_CUSTOM_TARGET(dist
						COMMAND make clean
						COMMAND rm -rf ${TMP}
						COMMAND mkdir -p ${TMP}
						COMMAND cp ${S}/build_mingw32 ${S}/build_mingw64 ${S}/CMakeLists.txt ${S}/COPYING.txt ${S}/README.txt ${S}/README ${TMP}
						COMMAND cp -r ${S}/cmake ${S}/main ${S}/src ${TMP}
						COMMAND rm -rf `find ${TMP} -name cmake_install.cmake` `find ${TMP} -name Makefile` `find ${TMP} -type d -name CMakeFiles` ${TMP}/CMakeCache.txt
						COMMAND rm ${TMP}/src/libs/macosx/libSDL-1.2.0.dylib
						COMMAND tar cjf rtcw-sp-${VERSION}.tar.bz2 ${TMP}
						COMMAND rm -rf ${TMP})


INCLUDE(InstallRequiredSystemLibraries)
SET(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Return to Castle Wolfenstein - Single Player")
SET(CPACK_PACKAGE_VENDOR "id Software LLC, a ZeniMax Media company")
SET(CPACK_PACKAGE_DESCRIPTION_FILE "${CMAKE_SOURCE_DIR}/README.txt")
SET(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_SOURCE_DIR}/COPYING.txt")

SET(CPACK_PACKAGE_VERSION_MAJOR "${VERSION_MAJOR}")
SET(CPACK_PACKAGE_VERSION_MINOR "${VERSION_MINOR}")
SET(CPACK_PACKAGE_VERSION_PATCH "${VERSION_PATCH}")
IF(VERSION_SUFFIX)
	SET(CPACK_PACKAGE_VERSION_PATCH "${VERSION_PATCH}-${VERSION_SUFFIX}")
ENDIF(VERSION_SUFFIX)
SET(CPACK_PACKAGE_INSTALL_DIRECTORY "Return to Castle Wolfenstein")
IF(WIN32)
#SET(CPACK_NSIS_MUI_ICON "${CMAKE_SOURCE_DIR}/resources\\\\rtcw-sp.ico")
SET(CPACK_NSIS_INSTALLED_ICON_NAME "rtcw-sp.exe")
SET(CPACK_NSIS_DISPLAY_NAME "Return to Castle Wolfenstein - Single Player")
SET(CPACK_NSIS_HELP_LINK "http://gitorious.org/rtcw/")
SET(CPACK_NSIS_URL_INFO_ABOUT "https://gitorious.org/rtcw/")
SET(CPACK_NSIS_CONTACT "tobydox@users.sourceforge.net")
SET(CPACK_PACKAGE_EXECUTABLES "rtcw-sp.exe;Return to Castle Wolfenstein - Single Player")
SET(CPACK_NSIS_MENU_LINKS "rtcw-sp.exe;Return to Castle Wolfenstein - Single Player")
ELSE(WIN32)
SET(CPACK_STRIP_FILES "bin/rtcw-sp")
SET(CPACK_PACKAGE_EXECUTABLES "rtcw-sp" "Return to Castle Wolfenstein - Single Player")
ENDIF(WIN32)

IF(WIN64)
	SET(CPACK_PACKAGE_FILE_NAME "rtcw-sp-${VERSION}-win64")
	SET(CPACK_NSIS_DEFINES "${CPACK_NSIS_DEFINES}
			InstallDir \\\"\\\$PROGRAMFILES64\\\\${CPACK_PACKAGE_INSTALL_DIRECTORY}\\\"")
ENDIF(WIN64)

#SET(MACOSX_BUNDLE_ICON_FILE "${CMAKE_SOURCE_DIR}/resources/logo.png")
SET(MACOSX_BUNDLE_GUI_IDENTIFIER "Return to Castle Wolfenstein - Single Player")
SET(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION}")
SET(MACOSX_BUNDLE_BUNDLE_NAME "Return to Castle Wolfenstein - Single Player")
SET(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION}")
SET(MACOSX_BUNDLE_BUNDLE_VERSION "${VERSION}")
SET(MACOSX_BUNDLE_COPYRIGHT "Tobias Doerffel, 2011")

SET(CPACK_SOURCE_GENERATOR "TBZ2")
SET(CPACK_SOURCE_PACKAGE_FILE_NAME "rtcw-sp-${VERSION}")
INCLUDE(CPack)

