cmake_minimum_required(VERSION 2.8)
project ( cmake_compile_qt_project )
set(PROJECT_NAME cmake_compile_qt_project_test)

# declaring files in your project

set ( SOURCES
 main.cpp
 mainwindow.cpp
)

set ( MOC_HEADERS
 mainwindow.h
)

set ( UIS
 mainwindow.ui
)

# set ( RESOURCES
# main.qrc
# )

# Next lines needed for building all Qt projects
find_package( Qt5 REQUIRED Core Gui Widgets)
set(QT_PREFIX C:/Qt/Qt5.9.6/5.9.6/msvc2015_64)
set(QT_USE_FILE ${QT_PREFIX}/lib/cmake/Qt5/Qt5Config.cmake)
include( ${QT_USE_FILE} )
add_definitions( ${QT_DEFINITIONS} )
include_directories( 
	${CMAKE_BINARY_DIR} 
	${QT_PREFIX}/include
	${QT_PREFIX}/include/QtWidgets
)


# Next, using precompiler, compiler and linker

# using Qt meta-system (precompiler)
QT5_ADD_RESOURCES( RES_SOURCES ${RESOURCES} )
QT5_WRAP_UI( UI_HEADERS ${UIS} )
QT5_WRAP_CPP( MOC_SRCS ${MOC_HEADERS} )

# Specify qt libraries path
link_directories(${QT_PREFIX}/lib)

# compile
add_executable( ${PROJECT_NAME} 
	${SOURCES} 
	${MOC_SRCS} 
	${RES_SOURCES} 
	${UI_HEADERS} 
)
# or use line below instead, if you using Windows ™ Operating System.
#add_executable( PROJECT_NAME WIN32 ${SOURCES} ${MOC_SRCS} ${RES_SOURCES} ${UI_HEADERS} )

# Remove the console of gui program
if(WIN32)
	if(MSVC)
		set_target_properties(${PROJECT_NAME} PROPERTIES
			WIN32_EXECUTABLE YES
			LINK_FLAGS "/ENTRY:mainCRTStartup"
		)
	elseif(CMAKE_COMPILER_IS_GNUCXX)
			# SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -mwindows") # Not tested
	else()
		message(SEND_ERROR "You are using an unsupported Windows compiler! (Not MSVC or GCC)")
	endif(MSVC)
elseif(APPLE)
	set_target_properties(${PROJECT_NAME} PROPERTIES
			MACOSX_BUNDLE YES
	)
elseif(UNIX)
	# Nothing special required
else()
	message(SEND_ERROR "You are on an unsupported platform! (Not Win32, Mac OS X or Unix)")
endif(WIN32)

# build it (link libraries)
target_link_libraries( ${PROJECT_NAME} 
	#${QT_LIBRARIES}
	Qt5Core
	Qt5Widgets
)

install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION bin)