# Modify only if you know what you are doing!
cmake_minimum_required(VERSION 2.8.12)
project(viso2)

###################
## find packages ##
###################
set(CMAKE_CXX_STANDARD 14)
find_package(catkin_simple REQUIRED)
find_package(OpenCV REQUIRED)

find_package(PNG REQUIRED)

include(FindPackageHandleStandardArgs)

set(png++_ROOT_DIR "" CACHE PATH "Folder contains png++")

find_path(png++_INCLUDE_DIR
    NAMES
        png++/color.hpp
        png++/config.hpp
        png++/consumer.hpp
        png++/convert_color_space.hpp
        png++/end_info.hpp
        png++/error.hpp
        png++/ga_pixel.hpp
        png++/generator.hpp
        png++/gray_pixel.hpp
        png++/image.hpp
        png++/image_info.hpp
        png++/index_pixel.hpp
        png++/info.hpp
        png++/info_base.hpp
        png++/io_base.hpp
        png++/packed_pixel.hpp
        png++/palette.hpp
        png++/pixel_buffer.hpp
        png++/pixel_traits.hpp
        png++/png.hpp
        png++/reader.hpp
        png++/require_color_space.hpp
        png++/rgb_pixel.hpp
        png++/rgba_pixel.hpp
        png++/streaming_base.hpp
        png++/tRNS.hpp
        png++/types.hpp
        png++/writer.hpp
    PATHS
        ${png++_ROOT_DIR}
    PATH_SUFFIXES
        src)

set(png++_INCLUDE_DIRS ${png++_INCLUDE_DIR} ${PNG_INCLUDE_DIRS})
set(png++_LIBRARIES ${PNG_LIBRARIES})

find_package_handle_standard_args(png++ DEFAULT_MSG
    png++_INCLUDE_DIR)

if(png++_FOUND)
    set(png++_INCLUDE_DIRS ${png++_INCLUDE_DIR})
    set(png++_LIBRARIES ${png++_LIBRARY})
endif()

catkin_simple(ALL_DEPS_REQUIRED)


add_library(${PROJECT_NAME} src/filter.cpp
src/matcher.cpp
src/matrix.cpp
src/reconstruction.cpp
src/triangle.cpp
src/viso_mono.cpp
src/viso_stereo.cpp
src/viso.cpp)
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBRARIES} ${catkin_LIBRARIES} ${png++_LIBRARIES})

cs_install()

cs_export()
