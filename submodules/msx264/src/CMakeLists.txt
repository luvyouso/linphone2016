############################################################################
# CMakeLists.txt
# Copyright (C) 2015  Belledonne Communications, Grenoble France
#
############################################################################
#
# This program is free software; you can redistribute it and/or
# modify it under the terms of the GNU General Public License
# as published by the Free Software Foundation; either version 2
# of the License, or (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA  02111-1307, USA.
#
############################################################################

set(LIBS
	${MEDIASTREAMER2_LIBRARIES}
	${X264_LIBRARIES}
)

set(SOURCE_FILES msx264.c)

if(ENABLE_STATIC)
	add_library(msx264-static STATIC ${SOURCE_FILES})
	set_target_properties(msx264-static PROPERTIES OUTPUT_NAME msx264)
	set_target_properties(msx264-static PROPERTIES LINKER_LANGUAGE CXX LINK_FLAGS "${LINK_FLAGS}")
	target_link_libraries(msx264-static ${LIBS})
	install(TARGETS msx264-static
		ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}/mediastreamer/plugins
		PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
	)
endif()
if(ENABLE_SHARED)
	add_library(msx264 MODULE ${SOURCE_FILES})
	set_target_properties(msx264 PROPERTIES LINKER_LANGUAGE CXX LINK_FLAGS "${LINK_FLAGS}")
	target_link_libraries(msx264 ${LIBS})
	if(MSVC)
		if(CMAKE_BUILD_TYPE STREQUAL "Debug")
			install(FILES ${CMAKE_CURRENT_BINARY_DIR}/Debug/libmsx264.pdb
				DESTINATION ${CMAKE_INSTALL_BINDIR}
				PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
			)
		endif()
		set_target_properties(msx264 PROPERTIES PREFIX "lib")
	endif()
	install(TARGETS msx264
		RUNTIME DESTINATION ${CMAKE_INSTALL_LIBDIR}/mediastreamer/plugins
		LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}/mediastreamer/plugins
		ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}/mediastreamer/plugins
		PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
	)
endif()
