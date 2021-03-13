cmake_minimum_required(VERSION 3.13)

project(ImGui)

set(imgui_headers
       imconfig.h
       imgui.h
       imgui_internal.h
       imstb_rectpack.h
       imstb_textedit.h
       imstb_truetype.h)

set(imgui_sources
       imgui.cpp
       imgui_demo.cpp
       imgui_draw.cpp
       imgui_widgets.cpp)

add_library(imgui ${imgui_sources})
target_sources(imgui PUBLIC ${imgui_headers})
target_include_directories(imgui INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})

set(imgui_backends allegro5;android;dx10;dx11;dx12;dx9;glfw;glut;marmalade;metal;opengl2;opengl3;osx;sdl;vulkan;wgpu;win32)
set(imgui_backend_files "")

foreach(backend IN LISTS imgui_backends)
    option(IMGUI_USE_BACKEND_${backend} "Use ${backend} as backend for ImGui." OFF)

    if(IMGUI_USE_BACKEND_${backend})
          if(${backend} STREQUAL "osx" OR ${backend} STREQUAL "metal")
              set(imgui_backend_files "${imgui_backend_files} ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_${backend}.h ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_${backend}.mm")
          else()
              set(imgui_backend_files "${imgui_backend_files} ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_${backend}.h ${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_${backend}.cpp")
          endif()
    endif()
endforeach()
message(${imgui_backend_files})

