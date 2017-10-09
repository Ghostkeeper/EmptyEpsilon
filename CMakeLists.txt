project(EmptyEpsilon)
cmake_minimum_required(VERSION 2.8.12)

option(ENABLE_CRASH_LOGGER "Enable the drmingw crash logging facilities" OFF)

set(CMAKE_MODULE_PATH
 ${CMAKE_CURRENT_SOURCE_DIR}/cmake
 ${CMAKE_MODULE_PATH}
)

if(DEFINED ENABLE_CRASH_LOGGER)
 if(WIN32)
  if(NOT DEFINED DRMINGW_ROOT)
   message(FATAL_ERROR "DRMINGW_ROOT was not set. Unable to continue")
  endif(NOT DEFINED DRMINGW_ROOT)
 endif(WIN32)
endif(DEFINED ENABLE_CRASH_LOGGER)

if(NOT DEFINED CPACK_PACKAGE_VERSION_MAJOR)
 string(TIMESTAMP CPACK_PACKAGE_VERSION_MAJOR "%Y")
 string(TIMESTAMP CPACK_PACKAGE_VERSION_MINOR "%m")
 string(TIMESTAMP CPACK_PACKAGE_VERSION_PATCH "%d")
endif(NOT DEFINED CPACK_PACKAGE_VERSION_MAJOR)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")

# install resources, scripts, and packs to app bundle instead of system dir.
if(APPLE)
set(CMAKE_INSTALL_PREFIX "./")
elseif(UNIX)
# Set RESOURCE_BASE_DIR on Unix so the built binary is able to find resources
add_definitions(-DRESOURCE_BASE_DIR="${CMAKE_INSTALL_PREFIX}/share/emptyepsilon/")
endif()

if(CONFIG_DIR)
    add_definitions(-DCONFIG_DIR="${CONFIG_DIR}")
elseif(UNIX)
    add_definitions(-DCONFIG_DIR="${CMAKE_INSTALL_PREFIX}/share/emptyepsilon/")
endif()

## ensure c++11 is used
if(NOT ${CMAKE_VERSION} VERSION_LESS 3.1)
    set(CMAKE_CXX_STANDARD 11)
else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
endif()

set(EXECUTABLE_NAME "EmptyEpsilon")
set(SOURCES
	src/main.cpp
	src/threatLevelEstimate.cpp
	src/preferenceManager.cpp
	src/pathPlanner.cpp
	src/epsilonServer.cpp
	src/particleEffect.cpp
	src/mouseCalibrator.cpp
	src/httpScriptAccess.cpp
	src/modelInfo.cpp
	src/packResourceProvider.cpp
	src/scienceDatabase.cpp
	src/commsScriptInterface.cpp
	src/modelData.cpp
	src/gameGlobalInfo.cpp
	src/script.cpp
	src/playerInfo.cpp
	src/gameStateLogger.cpp
	src/shipTemplate.cpp
	src/beamTemplate.cpp
	src/missileWeaponData.cpp
	src/factionInfo.cpp
	src/mesh.cpp
	src/scenarioInfo.cpp
	src/repairCrew.cpp
	src/GMScriptCallback.cpp
	src/tutorialGame.cpp
	src/menus/joinServerMenu.cpp
	src/menus/serverBrowseMenu.cpp
	src/menus/mainMenus.cpp
	src/menus/serverCreationScreen.cpp
	src/menus/tutorialMenu.cpp
	src/menus/optionsMenu.cpp
	src/menus/shipSelectionScreen.cpp
	src/menus/autoConnectScreen.cpp
	src/screens/cinematicViewScreen.cpp
	src/screens/crewStationScreen.cpp
	src/screens/topDownScreen.cpp
	src/screens/windowScreen.cpp
	src/screens/mainScreen.cpp
	src/screens/crew4/operationsScreen.cpp
	src/screens/crew4/engineeringAdvancedScreen.cpp
	src/screens/crew4/tacticalScreen.cpp
	src/screens/crew6/engineeringScreen.cpp
	src/screens/crew6/scienceScreen.cpp
	src/screens/crew6/relayScreen.cpp
	src/screens/crew6/weaponsScreen.cpp
	src/screens/crew6/helmsScreen.cpp
	src/screens/crew1/singlePilotScreen.cpp
	src/screens/extra/damcon.cpp
	src/screens/extra/powerManagement.cpp
	src/screens/extra/databaseScreen.cpp
	src/screens/extra/shipLogScreen.cpp
	src/screens/gm/gameMasterScreen.cpp
	src/screens/gm/chatDialog.cpp
	src/screens/gm/tweak.cpp
	src/screenComponents/aimLock.cpp
	src/screenComponents/alertOverlay.cpp
	src/screenComponents/helpOverlay.cpp
	src/screenComponents/missileTubeControls.cpp
	src/screenComponents/selfDestructIndicator.cpp
	src/screenComponents/viewport3d.cpp
	src/screenComponents/selfDestructEntry.cpp
	src/screenComponents/dockingButton.cpp
	src/screenComponents/shieldsEnableButton.cpp
	src/screenComponents/selfDestructButton.cpp
	src/screenComponents/shieldFreqencySelect.cpp
	src/screenComponents/jumpControls.cpp
	src/screenComponents/impulseControls.cpp
	src/screenComponents/frequencyCurve.cpp
	src/screenComponents/noiseOverlay.cpp
	src/screenComponents/powerDamageIndicator.cpp
	src/screenComponents/beamTargetSelector.cpp
	src/screenComponents/shipInternalView.cpp
	src/screenComponents/beamFrequencySelector.cpp
	src/screenComponents/radarView.cpp
	src/screenComponents/rawScannerDataRadarOverlay.cpp
	src/screenComponents/scanTargetButton.cpp
	src/screenComponents/snapSlider.cpp
	src/screenComponents/indicatorOverlays.cpp
	src/screenComponents/openCommsButton.cpp
	src/screenComponents/combatManeuver.cpp
	src/screenComponents/rotatingModelView.cpp
	src/screenComponents/shipDestroyedPopup.cpp
	src/screenComponents/warpControls.cpp
	src/screenComponents/targetsContainer.cpp
	src/screenComponents/globalMessage.cpp
	src/screenComponents/commsOverlay.cpp
	src/screenComponents/jumpIndicator.cpp
	src/screenComponents/scanningDialog.cpp
	src/screenComponents/signalQualityIndicator.cpp
	src/screenComponents/mainScreenControls.cpp
	src/screenComponents/databaseView.cpp
	src/screenComponents/shipsLogControl.cpp
	src/screenComponents/onScreenKeyboard.cpp
	src/screenComponents/hackingDialog.cpp
	src/screenComponents/customShipFunctions.cpp
	src/screenComponents/scrollingBanner.cpp
	src/gui/colorConfig.cpp
	src/gui/hotkeyConfig.cpp
	src/gui/mouseRenderer.cpp
	src/gui/scriptError.cpp
	src/gui/gui2_slider.cpp
	src/gui/gui2_togglebutton.cpp
	src/gui/gui2_arrow.cpp
	src/gui/gui2_selector.cpp
	src/gui/gui2_canvas.cpp
	src/gui/gui2_rotationdial.cpp
	src/gui/gui2_textentry.cpp
	src/gui/gui2_label.cpp
	src/gui/gui2_image.cpp
	src/gui/gui2_autolayout.cpp
	src/gui/gui2_arrowbutton.cpp
	src/gui/gui2_entrylist.cpp
	src/gui/gui2_progressbar.cpp
	src/gui/gui2_scrolltext.cpp
	src/gui/gui2_advancedscrolltext.cpp
	src/gui/gui2_button.cpp
	src/gui/gui2_resizabledialog.cpp
	src/gui/debugRenderer.cpp
	src/gui/gui2_element.cpp
	src/gui/gui2_keyvaluedisplay.cpp
	src/gui/gui2_listbox.cpp
	src/gui/gui2_scrollbar.cpp
	src/gui/gui2_container.cpp
	src/gui/gui2_panel.cpp
	src/gui/gui2_overlay.cpp
	src/spaceObjects/spaceStation.cpp
	src/spaceObjects/spaceship.cpp
	src/spaceObjects/wormHole.cpp
	src/spaceObjects/spaceObject.cpp
	src/spaceObjects/nebula.cpp
	src/spaceObjects/explosionEffect.cpp
	src/spaceObjects/cpuShip.cpp
	src/spaceObjects/asteroid.cpp
	src/spaceObjects/mine.cpp
	src/spaceObjects/blackHole.cpp
	src/spaceObjects/missileWeapon.cpp
	src/spaceObjects/EMPMissile.cpp
	src/spaceObjects/playerSpaceship.cpp
	src/spaceObjects/beamEffect.cpp
	src/spaceObjects/homingMissile.cpp
	src/spaceObjects/hvli.cpp
	src/spaceObjects/electricExplosionEffect.cpp
	src/spaceObjects/supplyDrop.cpp
	src/spaceObjects/warpJammer.cpp
	src/spaceObjects/scanProbe.cpp
	src/spaceObjects/nuke.cpp
	src/spaceObjects/artifact.cpp
	src/spaceObjects/shipTemplateBasedObject.cpp
	src/spaceObjects/planet.cpp
	src/spaceObjects/spaceshipParts/beamWeapon.cpp
	src/spaceObjects/spaceshipParts/weaponTube.cpp
	src/ai/fighterAI.cpp
	src/ai/ai.cpp
	src/ai/aiFactory.cpp
	src/ai/missileVolleyAI.cpp
	src/hardware/dmx512SerialDevice.cpp
	src/hardware/enttecDMXProDevice.cpp
	src/hardware/sACNDMXDevice.cpp
	src/hardware/uDMXDevice.cpp
	src/hardware/hardwareController.cpp
	src/hardware/hardwareMappingEffects.cpp
	src/hardware/hardwareOutputDevice.cpp
	src/hardware/serialDriver.cpp
	src/hardware/virtualOutputDevice.cpp
)
if(WIN32)
    list(APPEND SOURCES EmptyEpsilon.rc)
endif()
add_definitions(-DWINDOW_TITLE="EmptyEpsilon")
add_definitions(-DVERSION_NUMBER=${CPACK_PACKAGE_VERSION_MAJOR}${CPACK_PACKAGE_VERSION_MINOR}${CPACK_PACKAGE_VERSION_PATCH})
if(ENABLE_CRASH_LOGGER)
  add_definitions(-DENABLE_CRASH_LOGGER)
  if(WIN32)
    link_directories(${DRMINGW_ROOT}/lib/)
  endif()
endif()

if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework OpenGL -framework Foundation")
endif()

add_executable(${EXECUTABLE_NAME} MACOSX_BUNDLE ${SOURCES})

include_directories(${CMAKE_SOURCE_DIR}/src)

## Setup Serious Proton and its included sublibraries
find_package(seriousproton)
find_package(lua)
find_package(box2d)
find_package(glew)
if(NOT SERIOUS_PROTON_FOUND OR NOT LUA_FOUND OR NOT BOX2D_FOUND OR NOT GLEW_FOUND)
 message(STATUS "Building seriousproton, lua, box2d and glew from scratch.")
 include(ExternalProject)
 ExternalProject_Add(seriousproton
  GIT_REPOSITORY ~/Projects/SeriousProton #TODO: temporary for testing
  GIT_TAG feature_cmake
  CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}
 )
 set(SERIOUS_PROTON_LIB ${CMAKE_INSTALL_PREFIX}/lib/${CMAKE_STATIC_LIBRARY_PREFIX}seriousproton${CMAKE_STATIC_LIBRARY_SUFFIX})
 set(SERIOUS_PROTON_INCLUDE_DIR ${CMAKE_INSTALL_PREFIX}/include/seriousproton)
 set(LUA_LIB ${CMAKE_INSTALL_PREFIX}/lib/${CMAKE_STATIC_LIBRARY_PREFIX}lua${CMAKE_STATIC_LIBRARY_SUFFIX})
 set(LUA_INCLUDE_DIR ${CMAKE_INSTALL_PREFIX}/include/lua)
 set(BOX2D_LIB ${CMAKE_INSTALL_PREFIX}/lib/${CMAKE_STATIC_LIBRARY_PREFIX}box2d${CMAKE_STATIC_LIBRARY_SUFFIX})
 set(BOX2D_INCLUDE_DIR ${CMAKE_INSTALL_PREFIX}/include/Box2D)
 set(GLEW_LIB ${CMAKE_INSTALL_PREFIX}/lib/${CMAKE_STATIC_LIBRARY_PREFIX}glew${CMAKE_STATIC_LIBRARY_SUFFIX})
 set(GLEW_INCLUDE_DIR ${CMAKE_INSTALL_PREFIX}/include/glew)
 add_dependencies(${EXECUTABLE_NAME} seriousproton)
endif()
link_directories(${CMAKE_INSTALL_PREFIX}/lib)
target_link_libraries(${EXECUTABLE_NAME} ${SERIOUS_PROTON_LIB} ${LUA_LIB} ${BOX2D_LIB} ${GLEW_LIB})
include_directories(${SERIOUS_PROTON_INCLUDE_DIR} ${LUA_INCLUDE_DIR} ${BOX2D_INCLUDE_DIR} ${GLEW_INCLUDE_DIR})

## Setup SFML
find_package(SFML 2.3 COMPONENTS system window graphics network audio)
if(NOT ${SFML_FOUND})
 message(STATUS "Couldn't find SFML. Building it from scratch. Installing to ${CMAKE_INSTALL_PREFIX}")
 set(SFML_PATCH_COMMAND "")
 if(${UNIX})
  ## On GCC in Ubuntu there's a bug with linking to SFML. See https://en.sfml-dev.org/forums/index.php?topic=20638.0 for details.
  SET(SFML_PATCH_COMMAND patch -p0 ${CMAKE_CURRENT_BINARY_DIR}/SFML-prefix/src/SFML/src/SFML/Graphics/CMakeLists.txt < ${CMAKE_CURRENT_SOURCE_DIR}/cmake/SFMLonGCC53.patch)
 endif()
 include(ExternalProject)
 ExternalProject_Add(SFML
  GIT_REPOSITORY https://github.com/SFML/SFML.git
  GIT_TAG 2.4.2 #Always build with this tag, so we're sure it is stable.
  PATCH_COMMAND ${SFML_PATCH_COMMAND}
  CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}
 )
 set(SFML_INCLUDE_DIR ${CMAKE_INSTALL_PREFIX}/include) #Otherwise this is filled by FindSFML.cmake.
 set(SFML_LIBRARIES
  ${CMAKE_INSTALL_PREFIX}/lib/${CMAKE_SHARED_LIBRARY_PREFIX}sfml-audio${CMAKE_SHARED_LIBRARY_SUFFIX}
  ${CMAKE_INSTALL_PREFIX}/lib/${CMAKE_SHARED_LIBRARY_PREFIX}sfml-graphics${CMAKE_SHARED_LIBRARY_SUFFIX}
  ${CMAKE_INSTALL_PREFIX}/lib/${CMAKE_SHARED_LIBRARY_PREFIX}sfml-network${CMAKE_SHARED_LIBRARY_SUFFIX}
  ${CMAKE_INSTALL_PREFIX}/lib/${CMAKE_SHARED_LIBRARY_PREFIX}sfml-system${CMAKE_SHARED_LIBRARY_SUFFIX}
  ${CMAKE_INSTALL_PREFIX}/lib/${CMAKE_SHARED_LIBRARY_PREFIX}sfml-window${CMAKE_SHARED_LIBRARY_SUFFIX}
 )
 add_dependencies(${EXECUTABLE_NAME} SFML)
endif()
include_directories(${SFML_INCLUDE_DIR})
target_link_libraries(${EXECUTABLE_NAME} ${SFML_LIBRARIES})

# Setup OpenGl
find_package(OpenGL REQUIRED)
include_directories(${OPENGL_INCLUDE_DIR})
target_link_libraries(${EXECUTABLE_NAME} ${OPENGL_LIBRARIES})

# Some magical Win 32 stuff
if(WIN32)
  target_link_libraries(${EXECUTABLE_NAME} wsock32 iphlpapi)
  if(ENABLE_CRASH_LOGGER)
    include_directories(${DRMINGW_ROOT}/include/)
    target_link_libraries(${EXECUTABLE_NAME} exchndl)
  endif()
elseif(APPLE)
  set_target_properties(${EXECUTABLE_NAME} PROPERTIES MACOSX_BUNDLE_INFO_PLIST ${CMAKE_SOURCE_DIR}/osx/MacOSXBundleInfo.plist.in)
  set_target_properties(${EXECUTABLE_NAME} PROPERTIES MACOSX_BUNDLE_ICON_FILE "${EXECUTABLE_NAME}.icns")
endif()


include(InstallRequiredSystemLibraries)

if(WIN32)
  install(FILES ${MING_DLL_PATH}/libstdc++-6.dll DESTINATION .)
  install(FILES ${MING_DLL_PATH}/libgcc_s_sjlj-1.dll DESTINATION .)
  install(FILES /usr/i686-w64-mingw32/lib/libwinpthread-1.dll DESTINATION .)
  install(FILES ${SFML_ROOT}/bin/openal32.dll DESTINATION .)
  install(FILES ${SFML_ROOT}/bin/sfml-audio-2.dll DESTINATION .)
  install(FILES ${SFML_ROOT}/bin/sfml-graphics-2.dll DESTINATION .)
  install(FILES ${SFML_ROOT}/bin/sfml-network-2.dll DESTINATION .)
  install(FILES ${SFML_ROOT}/bin/sfml-system-2.dll DESTINATION .)
  install(FILES ${SFML_ROOT}/bin/sfml-window-2.dll DESTINATION .)
  if(ENABLE_CRASH_LOGGER)
    install(FILES ${DRMINGW_ROOT}/bin/dbghelp.dll DESTINATION .)
    install(FILES ${DRMINGW_ROOT}/bin/exchndl.dll DESTINATION .)
    install(FILES ${DRMINGW_ROOT}/bin/mgwhelp.dll DESTINATION .)
    install(FILES ${DRMINGW_ROOT}/bin/symsrv.dll DESTINATION .)
    install(FILES ${DRMINGW_ROOT}/bin/symsrv.yes DESTINATION .)
  endif()

  install(DIRECTORY resources DESTINATION .)
  install(DIRECTORY scripts DESTINATION .)
  install(DIRECTORY packs DESTINATION .)
  install(TARGETS ${EXECUTABLE_NAME} RUNTIME DESTINATION .)
elseif(APPLE)
  install(FILES logo.icns DESTINATION "EmptyEpsilon.app/Contents/Resources" RENAME "${EXECUTABLE_NAME}.icns")

  install(DIRECTORY resources DESTINATION "EmptyEpsilon.app/Contents/Resources")
  install(DIRECTORY scripts DESTINATION "EmptyEpsilon.app/Contents/Resources")
  install(DIRECTORY packs DESTINATION "EmptyEpsilon.app/Contents/Resources")
else()
  include(GNUInstallDirs)
  install(DIRECTORY resources DESTINATION "${CMAKE_INSTALL_FULL_DATAROOTDIR}/emptyepsilon/")
  install(DIRECTORY scripts DESTINATION "${CMAKE_INSTALL_FULL_DATAROOTDIR}/emptyepsilon/")
  install(DIRECTORY packs DESTINATION "${CMAKE_INSTALL_FULL_DATAROOTDIR}/emptyepsilon/")
  install(TARGETS ${EXECUTABLE_NAME} RUNTIME DESTINATION ${CMAKE_INSTALL_FULL_BINDIR})
endif()

find_package(PythonInterp)
if(PYTHONINTERP_FOUND)
  add_custom_target(scriptdoc ALL ${PYTHON_EXECUTABLE} ${CMAKE_SOURCE_DIR}/compile_script_docs.py ${CMAKE_BINARY_DIR}/script_reference.html WORKING_DIRECTORY ${CMAKE_SOURCE_DIR} COMMENT "Building script reference documentation.")
  install(FILES ${CMAKE_BINARY_DIR}/script_reference.html DESTINATION .)
endif()

set(CPACK_PACKAGE_EXECUTABLES ${EXECUTABLE_NAME})
set(CPACK_GENERATOR "ZIP")
include(CPack)
