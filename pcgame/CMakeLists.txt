cmake_minimum_required(VERSION 3.15)
project(ShelterProtocol CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(ShelterProtocol
    src/main.cpp
)

# --------------------------------------------------------
#  GLFW - windowing/input. Provided by MSYS2 package
#  mingw-w64-x86_64-glfw in the GitHub Actions workflow.
# --------------------------------------------------------
find_package(glfw3 REQUIRED)
target_link_libraries(ShelterProtocol PRIVATE glfw)

# --------------------------------------------------------
#  GLAD - OpenGL function loader. Provided by MSYS2 package
#  mingw-w64-x86_64-glad in the GitHub Actions workflow.
# --------------------------------------------------------
find_package(glad REQUIRED)
target_link_libraries(ShelterProtocol PRIVATE glad::glad)

# OpenGL itself
find_package(OpenGL REQUIRED)
target_link_libraries(ShelterProtocol PRIVATE OpenGL::GL)

# --------------------------------------------------------
#  Copy shaders next to the executable at build time so the
#  game can find them without extra setup.
# --------------------------------------------------------
add_custom_command(TARGET ShelterProtocol POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_SOURCE_DIR}/shaders
        $<TARGET_FILE_DIR:ShelterProtocol>/shaders
)

if (WIN32)
    target_compile_definitions(ShelterProtocol PRIVATE _USE_MATH_DEFINES)
endif()
