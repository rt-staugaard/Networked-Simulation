cmake_minimum_required(VERSION 3.23)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(ray-tracer VERSION 1.0)

find_package(glfw3 REQUIRED)
include_directories(${CMAKE_SOURCE_DIR})

# GLAD LIBRARY
add_library(GLAD dependencies/glad/src/glad.c)
target_include_directories(GLAD PUBLIC dependencies/glad/include)

# WINDOW MODULE
add_library(WindowModule INTERFACE)
target_sources(WindowModule INTERFACE modules/graphics/window.hpp)
target_link_libraries(WindowModule INTERFACE GLAD)
target_link_libraries(WindowModule INTERFACE glfw)

# GRAPHICS MODULE
add_library(GraphicsModule INTERFACE)
target_sources(GraphicsModule INTERFACE modules/graphics/DisplayDrawer.hpp)
target_link_libraries(GraphicsModule INTERFACE GLAD)
target_include_directories(GraphicsModule INTERFACE dependencies/shader)

# INPUT MODULE
add_library(InputModule INTERFACE)
target_sources(InputModule INTERFACE modules/input/inputHandling.hpp)
target_include_directories(InputModule INTERFACE glfw)
target_include_directories(InputModule INTERFACE modules/input/interaction.hpp)

# PHYSICS MODULE
add_library(PhysicsModule INTERFACE)
target_sources(PhysicsModule INTERFACE modules/physics/physics.hpp)
target_include_directories(PhysicsModule INTERFACE modules/physics/config.hpp)

# NETWORK MODULE 1: SERVER
add_library(ServerModule INTERFACE)
target_sources(ServerModule INTERFACE modules/network/server.hpp)
target_include_directories(ServerModule INTERFACE modules/input/interaction.hpp)

# NETWORK MODULE 2: CLIENT
add_library(ClientModule INTERFACE)
target_sources(ClientModule INTERFACE modules/network/client.hpp)
target_include_directories(ClientModule INTERFACE modules/input/interaction.hpp)


add_executable(server src/server/server.cpp)
target_link_libraries(server PRIVATE 
    ServerModule
    PhysicsModule
)
set_target_properties(server PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/server")

add_executable(client src/client/client.cpp)
target_link_libraries(client PRIVATE 
    ClientModule
    WindowModule
    GraphicsModule
    InputModule
)
target_include_directories(client INTERFACE modules/physics)
set_target_properties(client PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/client")


