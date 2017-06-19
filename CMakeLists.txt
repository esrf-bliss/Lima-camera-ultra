############################################################################
# This file is part of LImA, a Library for Image Acquisition
#
# Copyright (C) : 2009-2017
# European Synchrotron Radiation Facility
# BP 220, Grenoble 38043
# FRANCE
#
# This is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 3 of the License, or
# (at your option) any later version.
#
# This software is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, see <http://www.gnu.org/licenses/>.
############################################################################
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
