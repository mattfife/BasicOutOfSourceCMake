cmake_minimum_required(VERSION 3.13)

# add header files
set( HEADER_FILES 
	main.h
)

set( SRC_FILES
	main.cpp
)

# add the executable
add_executable(helloworld ${HEADER_FILES} ${SRC_FILES})

# Add link libraries
#target_link_libraries(helloworld anylib.lib)