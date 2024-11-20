cmake_minimum_required(VERSION 3.12)

project(ProjetoFinalCG)

# Ativa o modo C++17
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Adiciona o executável do projeto
add_executable(ProjetoFinalCG "main.cpp" )

# Configuração do CMake para encontrar GLM
set(GLM_INCLUDE_DIR "${CMAKE_SOURCE_DIR}/deps/glm")
target_include_directories(ProjetoFinalCG PRIVATE "${GLM_INCLUDE_DIR}")

# Configuração do GLFW
set(GLFW_INCLUDE_DIR "${CMAKE_SOURCE_DIR}/deps/glfw/include")
set(GLFW_LIBRARY_DIR "${CMAKE_SOURCE_DIR}/deps/glfw/lib-vc2019")
target_include_directories(ProjetoFinalCG PRIVATE "${GLFW_INCLUDE_DIR}")
target_link_libraries(ProjetoFinalCG PRIVATE "${GLFW_LIBRARY_DIR}/glfw3.lib")

# Configuração do GLEW
set(GLEW_INCLUDE_DIR "${CMAKE_SOURCE_DIR}/deps/glew/include")
set(GLEW_LIBRARY_DIR "${CMAKE_SOURCE_DIR}/deps/glew/lib/Release/x64")
target_include_directories(ProjetoFinalCG PRIVATE "${GLEW_INCLUDE_DIR}")

# Usa o caminho completo para glew32.lib
target_link_libraries(ProjetoFinalCG PRIVATE "${GLEW_LIBRARY_DIR}/glew32.lib")

# Configuração do STB
set(STB_INCLUDE_DIR "${CMAKE_SOURCE_DIR}/deps/stb")
target_include_directories(ProjetoFinalCG PRIVATE "${STB_INCLUDE_DIR}")

# Configuração do Assimp
set(ASSIMP_ROOT "${CMAKE_SOURCE_DIR}/deps/assimp")
set(ASSIMP_INCLUDE_DIR "${ASSIMP_ROOT}/include")
set(ASSIMP_LIBRARY_DIR "${ASSIMP_ROOT}/lib")
set(ASSIMP_DLL_DIR "${ASSIMP_ROOT}/bin")

if(EXISTS "${ASSIMP_INCLUDE_DIR}" AND EXISTS "${ASSIMP_LIBRARY_DIR}")
    message(STATUS "Assimp include directory: ${ASSIMP_INCLUDE_DIR}")
    message(STATUS "Assimp library directory: ${ASSIMP_LIBRARY_DIR}")
    target_include_directories(ProjetoFinalCG PRIVATE "${ASSIMP_INCLUDE_DIR}")
    target_link_libraries(ProjetoFinalCG PRIVATE "${ASSIMP_LIBRARY_DIR}/assimp-vc143-mt.lib")
else()
    message(FATAL_ERROR "Assimp não encontrado nos diretórios especificados: ${ASSIMP_INCLUDE_DIR} ou ${ASSIMP_LIBRARY_DIR}")
endif()

# Liga a biblioteca OpenGL
target_link_libraries(ProjetoFinalCG PRIVATE opengl32.lib)

# Copia as DLLs necessárias para o diretório de saída
add_custom_command(TARGET ProjetoFinalCG POST_BUILD
                   COMMAND ${CMAKE_COMMAND} -E copy
                   "${CMAKE_SOURCE_DIR}/deps/glew/bin/Release/x64/glew32.dll"
                   "$<TARGET_FILE_DIR:ProjetoFinalCG>/glew32.dll")
                   
add_custom_command(TARGET ProjetoFinalCG POST_BUILD
                   COMMAND ${CMAKE_COMMAND} -E copy_directory
                   "${CMAKE_SOURCE_DIR}/deps/assimp/bin"
                   "${CMAKE_BINARY_DIR}")
