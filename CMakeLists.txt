# CMakeLists.txt for Ultra camera

SET(CMAKE_SHARED_LINKER_FLAGS "-Wl,--add-needed")

#SOURCEs
SET(ultra_srcs src/UltraCamera.cpp  src/UltraInterface.cpp
	 src/UltraDetInfoCtrlObj.cpp src/UltraSyncCtrlObj.cpp
	 src/UltraNet.cpp)

FILE(STRINGS "VERSION" ultra_vers)
ADD_LIBRARY(limaultra SHARED ${ultra_srcs})

#INCLUDEs
TARGET_INCLUDE_DIRECTORIES(limaultra PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")
TARGET_INCLUDE_DIRECTORIES(limaultra PUBLIC "${CMAKE_SOURCE_DIR}/camera/common/espia/include")

#LIBRARIES

TARGET_LINK_LIBRARIES(limaultra limacore)


SET_TARGET_PROPERTIES(limaultra PROPERTIES VERSION ${ultra_vers} SOVERSION "1")

INSTALL(TARGETS limaultra LIBRARY DESTINATION lib)

IF(COMPILE_SIP)
    SET(NAME "ultra")
    include(CameraRunSIP)
    INSTALL(FILES python/__init__.py DESTINATION "${PYTHON_SITE_PACKAGES_DIR}/Lima/Ultra")
ENDIF()
