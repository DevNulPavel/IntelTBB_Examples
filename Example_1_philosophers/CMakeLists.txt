cmake_minimum_required(VERSION 3.14)

set (PROJECT IntelTBB_Test)
project(${PROJECT})

# Поиск IntelTBB в системе
# https://weekly-geekly.github.io/articles/335346/index.html
# https://stackoverflow.com/questions/48978579/how-can-i-fix-tbb-on-cmake
# https://stackoverflow.com/a/48978760
include("../cmake/FindTBB_1.cmake")
find_package(TBB REQUIRED)
if(TBB_FOUND)
	message("IntelTBB is found")
	message("IntelTBB include: ${TBB_INCLUDE_DIRS}")
	message("IntelTBB lib: ${TBB_LIBRARIES}") # ${TBB_IMPORTED_TARGETS}

	include_directories("${TBB_INCLUDE_DIRS}/.")
else()
	message(FATAL_ERROR "IntelTBB is not found")
endif()

# Стандарт С++
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
#set(CMAKE_REQUIRED_FLAGS -std=c++2a)
#remove_definitions(-std=c++14)
#remove_definitions(-std=gnu++14)
#add_definitions(-std=c++2a -fcoroutines-ts)

# Флаги
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -O0 -Wall -Wextra")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -O0 -Wall -Wextra")

# Список исходников
set (HEADERS 
		"")
set (SOURCES
		"src/main.cpp")

# создаем группу, чтобы заголовочники и исходники были в одной папке
source_group("Sources" FILES ${HEADERS} ${SOURCES})

# исполняемый файл
add_executable(${PROJECT} ${HEADERS} ${SOURCES})

# Линкуем с TBB
target_link_libraries(${PROJECT} ${TBB_LIBRARIES})

# Стандарт С++
#set_property(TARGET ${PROJECT} PROPERTY CXX_STANDARD 20)