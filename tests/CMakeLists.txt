cmake_minimum_required(VERSION 3.2)

######################################

#check GLIB module if exist
INCLUDE(FindPkgConfig)
PKG_CHECK_MODULES(GLIB REQUIRED  glib-2.0)

######################################

FIND_PACKAGE(Freetype REQUIRED)

#FIND_PATH(FRIBIDI_INCLUDE_DIR fribidi/fribidi.h
 # /usr/local/include
  #/usr/include
  #)
#pkg_check_modules(PC_HARFBUZZ REQUIRED harfbuzz>=0.9.20)


include_directories(
	#${HARFBUZZ_INCLUDE_DIRS}
	#${FRIBIDI_INCLUDE_DIR}/fribidi	
	${GLIB_INCLUDE_DIRS}	
	${FREETYPE_INCLUDE_DIR_freetype2}
	${CMAKE_CURRENT_SOURCE_DIR}/fonts/sha1sum
	${CMAKE_CURRENT_SOURCE_DIR}
 )

add_subdirectory (fonts)

#enable C99 mode
add_definitions(-std=c99)

#set the variable TEST_SRCS to the paths of the test files 
set(TEST_SRCS 
     ${CMAKE_CURRENT_SOURCE_DIR}/features-arabic.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/features-kerning.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/features-ligature.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/scripts-backward.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/scripts-backward-ltr.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/scripts-backward-rtl.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/scripts-forward.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/scripts-forward-ltr.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/scripts-forward-rtl.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/multi-fonts.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/multi-fonts2.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/xyoffset.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/test1.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/test2.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/test3.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/test4.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/test5.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/test1_LTR.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/test2_LTR.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/test3_LTR.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/test4_LTR.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/test5_LTR.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/test1_RTL.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/test2_RTL.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/test3_RTL.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/test4_RTL.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/test5_RTL.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/cursor_position1.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/cursor_position2.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/cursor_position3.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/cursor_position4.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/cursor_position_GB3.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/cursor_position_GB4.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/cursor_position_GB5.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/cursor_position_GB8a.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/cursor_position_GB9.test 
     ${CMAKE_CURRENT_SOURCE_DIR}/cursor_position_GB9a.test
)

#find the path of RAQM_INCLUDE_DIR
#FIND_PATH(RAQM_INCLUDE_DIR
#  /usr/local/include
#  /usr/include
#)

#find RAQM_LIBRARY
FIND_LIBRARY(RAQM_LIBRARY
  NAMES raqm
  PATHS /usr/local/lib /usr/lib
)

#set(CMAKE_C_COMPILER_INIT gcc)
#message("path : ${RAQM_LIBRARY}")

add_executable(raqm-test raqm-test.c)

#link the executable file with libraries
target_link_libraries(raqm-test 
	${RAQM_LIBRARY} 
	${FREETYPE_LIBRARIES}
	${GLIB_LIBRARIES}
	#${FRIBIDI_LIBRARY}
	#${HARFBUZZ_LIBRARIES}	
)


#set the variable SHELL_EXECUTABLE to certain path
set(SHELL_EXECUTABLE ${CMAKE_CURRENT_SOURCE_DIR}/runtest)


#copy the executable file "raqm-test" from build dir to the src dir after calling cmake 
add_custom_command(TARGET raqm-test
		POST_BUILD
		COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_BINARY_DIR}/raqm-test ${CMAKE_CURRENT_SOURCE_DIR}
)


#run "make check" for testing
 add_custom_target(check #ALL
                   COMMAND  sh ${SHELL_EXECUTABLE} ${TEST_SRCS}
		   WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
)


###########################################

#include(CTest)

#Setup CMake to run tests
#enable_testing()

#Run through each source
#foreach(testSrc ${TEST_SRCS})
        #Extract the filename without an extension (NAME_WE)
#        get_filename_component(testName ${testSrc} NAME_WE)

 #       add_test(
#		 NAME ${testName} 
 #                WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
  #               COMMAND sh ${SHELL_EXECUTABLE} ${TEST_SRCS}
#)

#set_tests_properties (${testName} PROPERTIES PASS_REGULAR_EXPRESSION "All tests passed.")

#endforeach(testSrc)

########################################

#file(COPY ${TEST_SRCS}  DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
#file(COPY ${SHELL_EXECUTABLE} DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
#file(GLOB TEST_FILES "${CMAKE_CURRENT_BINARY_DIR}/*.test")
