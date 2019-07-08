cmake_minimum_required(VERSION 3.5)

# Definition 
set(PROJECT_NAME terocr)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/bin")

set(CMAKE_CXX_COMPILER /usr/bin/g++)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")

# Include path
set(INCLUDE_DIRS ${INClUDE_DIRS} /usr/include/tesseract)
set(INCLUDE_DIRS ${INClUDE_DIRS} /usr/include/leptonica)

# Assign Include directory
include_directories(${INCLUDE_DIRS})
include(FindPkgConfig)

pkg_check_modules(LIBTESSERACT REQUIRED tesseract)
pkg_check_modules(LIBLEPTONICA REQUIRED lept)

foreach(FLAG ${LIBTESSERACT_CXXFLAGS_OTHER})
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${FLAG}")
endforeach()

foreach(FLAG ${LIBLEPTONICA_CXXFLAGS_OTHER})
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${FLAG}")
endforeach()

link_directories(${LIBTESSERACT_LIBRARY_DIRS})
link_directories(${LIBLEPTONICA_LIBRARY_DIRS})
# Find library source
file(GLOB_RECURSE LIB_SRC_FILES include/*/*.c)

# Build static/shared libray
#add_library(${PROJECT_NAME}_LIB STATIC ${LIB_SRC_FILES})

# Install library headers
file(GLOB_RECURSE LIB_INC_FILES include/*.h)
install(FILES ${LIB_INC_FILES} DESTINATION include)

file(GLOB_RECURSE SOURCE_FILES src/*.cpp)
add_executable(terrocr ${SOURCE_FILES})


# linked to the library
target_include_directories(terrocr PRIVATE ${LIBTESSERACT_INCLUDE_DIRS})
target_include_directories(terrocr PRIVATE ${LIBLEPTONICA_INCLUDE_DIRS})
target_link_libraries(terrocr PRIVATE ${LIBTESSERACT_LIBRARIES})
target_link_libraries(terrocr PRIVATE ${LIBLEPTONICA_LIBRARIES})
