cmake_minimum_required(VERSION 3.22)
project(esp CXX)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_VISIBILITY_PRESET hidden)

set(DOBBY_GENERATE_SHARED OFF CACHE BOOL "" FORCE)
add_subdirectory(third_party/Dobby dobby)

set(IMGUI third_party/imgui)
add_library(esp SHARED
  src/main.cpp
  ${IMGUI}/imgui.cpp ${IMGUI}/imgui_draw.cpp ${IMGUI}/imgui_tables.cpp ${IMGUI}/imgui_widgets.cpp
  ${IMGUI}/backends/imgui_impl_opengl3.cpp)
target_include_directories(esp PRIVATE ${IMGUI} ${IMGUI}/backends third_party/Dobby/include)
target_compile_definitions(esp PRIVATE IMGUI_IMPL_OPENGL_ES3)
target_link_libraries(esp PRIVATE dobby log EGL GLESv3 android dl)
target_link_options(esp PRIVATE -Wl,--exclude-libs,ALL)
