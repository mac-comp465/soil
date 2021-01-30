cmake_minimum_required (VERSION 3.9)
project (SOIL)

#aux_source_directory(${CMAKE_SOURCE_DIR}/src SOURCEFILES)

set(SOURCEFILES src/image_DXT.c src/image_helper.c src/SOIL.c) # src/stb_image_aug.c)
set(HEADERFILES inc/SOIL/image_DXT.h inc/SOIL/image_helper.h inc/SOIL/SOIL.h inc/SOIL/stbi_DDS_aug_c.h inc/SOIL/stbi_DDS_aug.h inc/SOIL/stb_image.h inc/SOIL/stb_image_write.h)

set (CMAKE_VERBOSE_MAKEFILE TRUE)

# Used for compiling in subdirectories of the project
include_directories (
  .
  src
  inc
)


############################################################
#OS Specific flags should go here
############################################################

if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
  set(CMAKE_XCODE_ATTRIBUTE_CLANG_CXX_LANGUAGE_STANDARD "c++11")
  set(CMAKE_XCODE_ATTRIBUTE_CLANG_CXX_LIBRARY "libc++")
  #set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -std=c++11 -stdlib=libc++")
endif()

if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
  # Linux specific code
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC -std=c++11") 
  add_definitions("-DOSC_HOST_LITTLE_ENDIAN")
endif (${CMAKE_SYSTEM_NAME} MATCHES "Linux")

if (${CMAKE_SYSTEM_NAME} MATCHES "Windows")
  # Windows specific code
endif (${CMAKE_SYSTEM_NAME} MATCHES "Windows")


############################################################
#Finally add all the targets here
############################################################

add_library(SOIL ${SOURCEFILES} ${HEADERFILES})

#------------------------------------------
# Install Target
#------------------------------------------

install( TARGETS ${PROJECT_NAME}
         LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
         ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
         RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)

install(DIRECTORY ${PROJECT_SOURCE_DIR}/inc/ DESTINATION "${CMAKE_INSTALL_PREFIX}/include")