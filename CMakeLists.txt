cmake_minimum_required(VERSION 3.20.0)
project(planning)
set(CMAKE_CXX_STANDARD 11)
#-------------------------动态库和可执行文件的目录指定-------------------------------------
#指定动态库和exe生成在bin目录(可执行文件运行需要加载动态库，动态库需要放在可执行文件的同一个目录)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)
#CMAKE_LIBRARY_OUTPUT_DIRECTORY(linux)
#CMAKE_ARCHIVE_OUTPUT_DIRECTORY(静态库，只会把库的位置制定出来)


#-------------------------生成动态库，以及依赖的头文件目录-------------------------------------
aux_source_directory(${PROJECT_SOURCE_DIR}/src SRC_LISTS)
#src下面的.cpp文件生成动态库，命名common
add_library(common 
SHARED
${SRC_LISTS}
)

#动态库依赖头文件,所以还有包含头文件目录
target_include_directories(common 
PUBLIC
${PROJECT_SOURCE_DIR}/include
)

#-------------------------生成可执行文件，以及依赖的头文件目录-------------------------------------
#可执行文件目录
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

#aux_source_directory(. SRX_LISTS)
#生成可执行文件，然后连接动态库
add_executable(planning_main planning_main.cpp)

#生成的可执行文件也要包含头文件
target_include_directories(planning_main
PUBLIC
${PROJECT_SOURCE_DIR}/include
)

#-------------------------可执行文件链接动态库-------------------------------------
#可执行文件链接到动态库上面
target_link_libraries(planning_main
PUBLIC
common
)

