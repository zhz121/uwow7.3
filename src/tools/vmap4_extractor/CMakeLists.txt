# Copyright (C) 2005-2009 MaNGOS project <http://getmangos.com/>
# Copyright (C) 2008-2015 TrinityCore <http://www.trinitycore.org/>
#
# This file is free software; as a special exception the author gives
# unlimited permission to copy and/or distribute it, with or without
# modifications, as long as this notice is preserved.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY, to the extent permitted by law; without even the
# implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

file(GLOB_RECURSE sources *.cpp *.h)

# uncomment next line to disable debug mode
add_definitions("-DIOMAP_DEBUG")

# build setup currently only supports libmpq 0.4.x
if( NOT MSVC )
  add_definitions("-Wall")
  add_definitions("-ggdb")
  add_definitions("-O3")
endif()

include_directories(
  ${CMAKE_SOURCE_DIR}/src/server/shared
  ${CMAKE_SOURCE_DIR}/dep/CascLib/src
  ${CMAKE_SOURCE_DIR}/dep/cppformat
)

add_executable(vmap4extractor ${sources})

target_link_libraries(vmap4extractor
  casc
  common
  ${BZIP2_LIBRARIES}
  ${ZLIB_LIBRARIES}
  ${Boost_LIBRARIES}
)

add_dependencies(vmap4extractor casc)

if( UNIX )
  install(TARGETS vmap4extractor DESTINATION bin)
elseif( WIN32 )
  install(TARGETS vmap4extractor DESTINATION "${CMAKE_INSTALL_PREFIX}")
endif()
