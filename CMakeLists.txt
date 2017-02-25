CMAKE_MINIMUM_REQUIRED(VERSION 3.8)
# -----------------------------------------------------------------------------------------
# Copy files from source directory to destination directory, substituting any
# variables.  Create destination directory if it does not exist.
macro(configure_files srcDir destDir)
    message(STATUS "Configuring directory ${destDir}")
    make_directory(${destDir})

    file(GLOB templateFiles RELATIVE ${srcDir} ${srcDir}/*)
    foreach(templateFile ${templateFiles})
        set(srcTemplatePath ${srcDir}/${templateFile})
        if(NOT IS_DIRECTORY ${srcTemplatePath})
            message(STATUS "Configuring file ${templateFile}")
            configure_file(
                    ${srcTemplatePath}
                    ${destDir}/${templateFile}
                    @ONLY)
        endif(NOT IS_DIRECTORY ${srcTemplatePath})
    endforeach(templateFile)
endmacro(configure_files)
# -----------------------------------------------------------------------------------------

# Incluir GLWF
ADD_SUBDIRECTORY("./lib/glfw-3.2.1")
LINK_LIBRARIES(glfw)
INCLUDE_DIRECTORIES(${glfw_INCLUDE_DIRS})
INCLUDE_DIRECTORIES("${GLFW_SOURCE_DIR}/deps/")

#Incluir ASSIMP
ADD_SUBDIRECTORY("./lib/assimp-3.3.1")
INCLUDE_DIRECTORIES(
    ${Assimp_SOURCE_DIR}/include
    ${Assimp_SOURCE_DIR}/code
)
TARGET_LINK_LIBRARIES(assimp ${ZLIB_LIBRARIES})

# Crear Proyecto
PROJECT(Red_Dragon)
ADD_EXECUTABLE(${PROJECT_NAME} "main.cpp" "${GLFW_SOURCE_DIR}/deps/glad.c")

# Copiar shaders
configure_files("./shaders" "shaders")

