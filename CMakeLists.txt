cmake_minimum_required(VERSION 3.6)
project(autobleem-gui)

set(CMAKE_CXX_STANDARD 11)
#set(CMAKE_VERBOSE_MAKEFILE ON)

message(STATUS "CMAKE_SYSTEM_PROCESSOR=${CMAKE_SYSTEM_PROCESSOR}")
if (CMAKE_SYSTEM_PROCESSOR MATCHES "^arm" )
	message(STATUS "Building for ARM")
	if(CMAKE_CXX_COMPILER_VERSION VERSION_GREATER 8.0)
		message(STATUS "CXX Compiler Version > 8")
		SET(CMAKE_C_FLAGS "-mfloat-abi=hard -march=armv8-a+simd -Os -s")
		SET(CMAKE_CXX_FLAGS " -mfloat-abi=hard -march=armv8-a+simd  -Os -s")
        message(STATUS "Newer GCC installed")
	else()
		message(STATUS "CXX Compiler Version <= 8")
		SET(CMAKE_C_FLAGS "-mfloat-abi=hard -march=armv7ve -s -Os -O3")
		SET(CMAKE_CXX_FLAGS "-mfloat-abi=hard -march=armv7ve -s -Os -O3")
	endif()

	include_directories(src/include "/opt/toolchain/armv8-sony-linux-gnueabihf/sysroot/usr/include")
	link_directories(/opt/toolchain/armv8-sony-linux-gnueabihf/sysroot/usr/lib)

else()

    message(STATUS "Building for PC CPU type=${CMAKE_BUILD_TYPE}")
	if( "${CMAKE_BUILD_TYPE}" STREQUAL "Debug" )
#		if (MEM_DEBUG)	# -DMEM_DEBUG=Extra in Clion does not work!  cmake doesn't see it

#			message(STATUS "Enabling all warnings")
#			# all compiler warnings
#			SET(CMAKE_C_FLAGS "-Wall -W")
#			SET(CMAKE_CXX_FLAGS "-Wall -W")

			message(STATUS "Enabling some warnings")
			# some compiler warnings
			SET(CMAKE_C_FLAGS "-Wreturn-type -Wuninitialized")
			SET(CMAKE_CXX_FLAGS "-Wreturn-type -Wuninitialized")

#			message(STATUS "Enabling additional memory debugging")
			# memory sanitizer with all compiler warnings
#			SET(CMAKE_C_FLAGS "-fsanitize=address -fno-omit-frame-pointer -fsanitize=undefined -ggdb -Wall -W")
#			SET(CMAKE_CXX_FLAGS "-fsanitize=address -fno-omit-frame-pointer -fsanitize=undefined -ggdb -Wall -W")

			# memory sanitizer with compiler warning for not returning a value from a function and uninitialized vars
#			SET(CMAKE_C_FLAGS "-fsanitize=address -fno-omit-frame-pointer -fsanitize=undefined -ggdb -Wreturn-type -Wuninitialized")
#			SET(CMAKE_CXX_FLAGS "-fsanitize=address -fno-omit-frame-pointer -fsanitize=undefined -ggdb -Wreturn-type -Wuninitialized")
			
			# memory sanitizer with no additonal compiler warnings
#			SET(CMAKE_C_FLAGS "-fsanitize=address -fno-omit-frame-pointer")
#			SET(CMAKE_CXX_FLAGS "-fsanitize=address -fno-omit-frame-pointer")

#			SET(LDFLAGS "-fsanitize=address -fno-omit-frame-pointer")

#		endif()
	else()
		message(STATUS "Building Release")
	endif()
endif()

find_package(Threads REQUIRED
             SDL2 REQUIRED)

add_library(sqlite3 STATIC libs/sqlite/sqlite3ab.c )

add_executable(
        autobleem-gui
        src/code/DebugTimer.cpp
        src/code/DebugTimer.h
        src/code/DirEntry.cpp
        src/code/DirEntry.h
        src/code/engine/cardedit.cpp
        src/code/engine/cardedit.h
        src/code/engine/cfgprocessor.cpp
        src/code/engine/cfgprocessor.h
        src/code/engine/config.cpp
        src/code/engine/config.h
        src/code/engine/coverdb.cpp
        src/code/engine/coverdb.h
        src/code/engine/database.cpp
        src/code/engine/database.h
        src/code/engine/ecmhelper.cpp
        src/code/engine/ecmhelper.h
        src/code/engine/game.cpp
        src/code/engine/game.h
        src/code/engine/GetGameDirHierarchy.cpp
        src/code/engine/GetGameDirHierarchy.h
        src/code/engine/inifile.cpp
        src/code/engine/inifile.h
        src/code/engine/isodir.cpp
        src/code/engine/isodir.h
        src/code/engine/memcard.cpp
        src/code/engine/memcard.h
        src/code/engine/metadata.cpp
        src/code/engine/metadata.h
        src/code/engine/padmapper.cpp
        src/code/engine/padmapper.h
        src/code/engine/scanner.cpp
        src/code/engine/scanner.h
        src/code/engine/serialscanner.cpp
        src/code/engine/serialscanner.h
        src/code/environment.cpp
        src/code/environment.h
        src/code/gui/gui.cpp
        src/code/gui/gui.h
        src/code/gui/gui_about.cpp
        src/code/gui/gui_about.h
        src/code/gui/gui_confirm.cpp
        src/code/gui/gui_confirm.h
        src/code/gui/gui_font.cpp
        src/code/gui/gui_font.h
        src/code/gui/gui_font_wrapper.h
        src/code/gui/gui_keyboard.cpp
        src/code/gui/gui_keyboard.h
        src/code/gui/gui_padconfig.cpp
        src/code/gui/gui_padconfig.h
        src/code/gui/gui_padTest.cpp
        src/code/gui/gui_padTest.h
        src/code/gui/gui_screen.cpp
        src/code/gui/gui_screen.h
        src/code/gui/gui_scrollWin.cpp
        src/code/gui/gui_scrollWin.h
        src/code/gui/gui_sdl_wrapper.cpp
        src/code/gui/gui_sdl_wrapper.h
        src/code/gui/gui_selectmemcard.cpp
        src/code/gui/gui_selectmemcard.h
        src/code/gui/gui_splash.cpp
        src/code/gui/gui_splash.h
        src/code/gui/menus/gui_gameDirMenu.h
        src/code/gui/menus/gui_gameEditorMenu.cpp
        src/code/gui/menus/gui_gameEditorMenu.h
        src/code/gui/menus/gui_gameManagerMenu.cpp
        src/code/gui/menus/gui_gameManagerMenu.h
        src/code/gui/menus/gui_memCardsMenu.cpp
        src/code/gui/menus/gui_memCardsMenu.h
        src/code/gui/menus/gui_menuBase.h
		src/code/gui/menus/gui_networkMenu.cpp
		src/code/gui/menus/gui_networkMenu.h 
        src/code/gui/menus/gui_optionsMenu.cpp
        src/code/gui/menus/gui_optionsMenu.h
        src/code/gui/menus/gui_optionsMenuBase.cpp 
        src/code/gui/menus/gui_optionsMenuBase.h
        src/code/gui/menus/gui_playlistsMenu.h
        src/code/gui/menus/gui_stringMenu.h
        src/code/gui/menus/gui_twoColumnStringMenu.h
        src/code/gui/starfx.cpp
        src/code/gui/starfx.h
        src/code/lang.cpp
        src/code/lang.h 
        src/code/launcher/emu_interceptor.cpp
        src/code/launcher/emu_interceptor.h
        src/code/launcher/gui_app_start.cpp 
        src/code/launcher/gui_app_start.h 
        src/code/launcher/gui_btn_guide.cpp
        src/code/launcher/gui_btn_guide.h
        src/code/launcher/gui_launcher.cpp
        src/code/launcher/gui_launcher.h 
        src/code/launcher/gui_launcher_loop.cpp
        src/code/launcher/gui_mc_manager.cpp
        src/code/launcher/gui_mc_manager.h
        src/code/launcher/gui_NotificationLine.cpp
        src/code/launcher/gui_NotificationLine.h
        src/code/launcher/launch_interceptor.cpp 
        src/code/launcher/launch_interceptor.h 
        src/code/launcher/pcsx_interceptor.cpp
        src/code/launcher/pcsx_interceptor.h
        src/code/launcher/ps_carousel.cpp
        src/code/launcher/ps_carousel.h
        src/code/launcher/ps_centerlabel.cpp
        src/code/launcher/ps_centerlabel.h
        src/code/launcher/ps_game.cpp 
        src/code/launcher/ps_game.h 
        src/code/launcher/ps_menu.cpp
        src/code/launcher/ps_menu.h
        src/code/launcher/ps_meta.cpp
        src/code/launcher/ps_meta.h
        src/code/launcher/ps_move_bnt.cpp
        src/code/launcher/ps_move_bnt.h 
        src/code/launcher/ps_obj.cpp
        src/code/launcher/ps_obj.h
        src/code/launcher/ps_settings_back.cpp 
        src/code/launcher/ps_settings_back.h
        src/code/launcher/ps_stateselector.cpp
        src/code/launcher/ps_stateselector.h
        src/code/launcher/ps_static.cpp
        src/code/launcher/ps_static.h
        src/code/launcher/ps_zoom_btn.cpp
        src/code/launcher/ps_zoom_btn.h
        src/code/launcher/ra_integrator.cpp
        src/code/launcher/ra_integrator.h
        src/code/launcher/retboot_interceptor.cpp
        src/code/launcher/retboot_interceptor.h
        src/code/main.cpp
        src/code/main.h
        src/code/unecm.c
        src/code/util.cpp
        src/code/util.h
        src/code/ver_migration.cpp
        src/code/ver_migration.h
		)

target_include_directories(autobleem-gui PUBLIC
		$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/libs/sqlite>
		$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/libs/nlohmann>
		)

add_executable(starter
		src/code/starter.cpp
		src/code/util.h
		src/code/util.cpp
		src/code/DirEntry.h
		src/code/DirEntry.cpp
        src/code/engine/inifile.cpp
        src/code/engine/memcard.cpp src/code/engine/memcard.h
		src/code/environment.h
		src/code/environment.cpp
		)

target_link_libraries(autobleem-gui
         Threads::Threads
         SDL2
         SDL2_image
         SDL2_mixer
         SDL2_ttf
		 sqlite3
         ${CMAKE_DL_LIBS}
)


#add_custom_command(TARGET autobleem-gui PRE_BUILD
#	   WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
#	   VERBATIM
#       COMMAND /bin/bash make_english.txt.sh
#	   )

add_custom_command(TARGET autobleem-gui PRE_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_SOURCE_DIR}/src/resources $<TARGET_FILE_DIR:autobleem-gui>)


configure_file(src/resources/default.lic default.lic COPYONLY)
configure_file(src/resources/default.png default.png COPYONLY)
configure_file(src/resources/pcsx.cfg pcsx.cfg COPYONLY)

