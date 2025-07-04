#---------------------------------------------------
# OSG CMAKE SUPPORT
# (C) by Michael Wagner, mtw@shared-reality.com 2005
# (C) Eric Wing, Luigi Calori and Robert Osfield 2006-2007
#---------------------------------------------------

PROJECT(OSG_PLUGINS_MASTER)

IF(NOT DYNAMIC_OPENSCENEGRAPH)
    ADD_DEFINITIONS(-DOSG_LIBRARY_STATIC)
ENDIF()

IF(NOT MSVC)
    SET(LIBRARY_OUTPUT_PATH "${LIBRARY_OUTPUT_PATH}/${OSG_PLUGINS}")
    SET(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/${OSG_PLUGINS}")
ENDIF()

SET(CMAKE_SHARED_MODULE_PREFIX ${OSG_PLUGIN_PREFIX})

IF(MSVC80 OR MSVC90)
    IF(NOT OSG_MSVC_GENERATE_PLUGINS_AND_WRAPPERS_MANIFESTS)
        SET(CMAKE_MODULE_LINKER_FLAGS "${CMAKE_MODULE_LINKER_FLAGS} /MANIFEST:NO")
    ENDIF()
ENDIF()

SET(TARGET_DEFAULT_PREFIX "osgdb_")
SET(TARGET_DEFAULT_LABEL_PREFIX "Plugins")
SET(TARGET_COMMON_LIBRARIES
    OpenThreads
    osg
    osgDB
    osgUtil
)

OPTION(BUILD_OSG_PLUGINS_BY_DEFAULT "If set, all available plugins are built by default and each plugin can be disabled from the build by setting BUILD_OSG_PLUGIN_<PLUGINNAME> to 0. If not set, no plugins are built by default and each desired plugin must be explicitly enabled by setting BUILD_OSG_PLUGIN_<PLUGINNAME> to 1." ON)

# Macro used for adding plugins below. By default, all plugins are built
# but can be disabled by setting the CMake Variable BUILD_OSG_PLUGIN_<PLUGINNAME> to 0.
macro(ADD_PLUGIN_DIRECTORY DIRECTORY)
    STRING(TOUPPER "${DIRECTORY}" DIRECTORY_UPPER)
    SET(VARNAME "BUILD_OSG_PLUGIN_${DIRECTORY_UPPER}")
    if (NOT DEFINED ${VARNAME})
        if (BUILD_OSG_PLUGINS_BY_DEFAULT)
            SET(${VARNAME} 1)
        else()
            SET(${VARNAME} 0)
        endif()
    endif()

    if ("${${VARNAME}}")
        ADD_SUBDIRECTORY("${DIRECTORY}")
    endif()
endmacro()

############################################################
#
#  NodeKit/Psudo loader plugins
#
ADD_PLUGIN_DIRECTORY(osga)
ADD_PLUGIN_DIRECTORY(rot)
ADD_PLUGIN_DIRECTORY(scale)
ADD_PLUGIN_DIRECTORY(trans)
ADD_PLUGIN_DIRECTORY(normals)
ADD_PLUGIN_DIRECTORY(revisions)
ADD_PLUGIN_DIRECTORY(view)
ADD_PLUGIN_DIRECTORY(shadow)
ADD_PLUGIN_DIRECTORY(terrain)

############################################################
#
#  Main native plugins
#
ADD_PLUGIN_DIRECTORY(osg)
ADD_PLUGIN_DIRECTORY(ive)

############################################################
#
#  Viewer plugins
#
ADD_PLUGIN_DIRECTORY(cfg)

############################################################
#
#  Shader plugins
#
ADD_PLUGIN_DIRECTORY(glsl)

############################################################
#
#  Image plugins
#
ADD_PLUGIN_DIRECTORY(rgb)
ADD_PLUGIN_DIRECTORY(bmp)
ADD_PLUGIN_DIRECTORY(pnm)
ADD_PLUGIN_DIRECTORY(dds)
ADD_PLUGIN_DIRECTORY(tga)
ADD_PLUGIN_DIRECTORY(hdr)
ADD_PLUGIN_DIRECTORY(dot)
ADD_PLUGIN_DIRECTORY(vtf)
ADD_PLUGIN_DIRECTORY(ktx)

# add by akuan begin >>>>>> [print message to make it more clearly checking 'zlib']
if(NOT DEFINED ZLIB_FOUND)
    if(ZLIB_INCLUDE_DIR AND ZLIB_LIBRARIES)
        set(ZLIB_FOUND TRUE)
    else()
        set(ZLIB_FOUND FALSE)
    endif()
endif()
if(ZLIB_FOUND)
    message(STATUS "zlib is found!")
else()
    message(WARNING "zlib is not found!")
endif()
# add end <<<<<<

IF(JPEG_FOUND)
    ADD_PLUGIN_DIRECTORY(jpeg)
ENDIF()
IF(JASPER_FOUND)
    ADD_PLUGIN_DIRECTORY(jp2)
ENDIF()
IF(OPENEXR_FOUND AND ZLIB_FOUND AND OSG_CPP_EXCEPTIONS_AVAILABLE)
    ADD_PLUGIN_DIRECTORY(exr)
ENDIF()
IF(GIFLIB_FOUND)
    ADD_PLUGIN_DIRECTORY(gif)
ENDIF()
IF(PNG_FOUND AND ZLIB_FOUND)
    ADD_PLUGIN_DIRECTORY(png)
ENDIF()
IF(TIFF_FOUND)
    ADD_PLUGIN_DIRECTORY(tiff)
ENDIF()
IF(GDAL_FOUND)
    ADD_PLUGIN_DIRECTORY(gdal)
    ADD_PLUGIN_DIRECTORY(ogr)
ENDIF()
IF(GTA_FOUND AND OSG_CPP_EXCEPTIONS_AVAILABLE)
    ADD_PLUGIN_DIRECTORY(gta)
ENDIF()


############################################################
#
# 3D Image plugins
#
IF(DCMTK_FOUND AND ZLIB_FOUND)
    ADD_PLUGIN_DIRECTORY(dicom)
ELSE()
    IF(ITK_FOUND)
        ADD_PLUGIN_DIRECTORY(dicom)
    ENDIF()
ENDIF()

############################################################
#
#  3rd party 3d plugins
#

ADD_PLUGIN_DIRECTORY(3dc)

ADD_PLUGIN_DIRECTORY(p3d)

IF(CURL_FOUND)
    ADD_PLUGIN_DIRECTORY(curl)
ENDIF()

IF(ZLIB_FOUND)
    ADD_PLUGIN_DIRECTORY(gz)
ENDIF()

IF(NOT OSG_GLES1_AVAILABLE AND NOT OSG_GLES2_AVAILABLE)
    IF(INVENTOR_FOUND)
        ADD_PLUGIN_DIRECTORY(Inventor)
    ENDIF()
ENDIF()

IF(COLLADA_FOUND AND OSG_CPP_EXCEPTIONS_AVAILABLE)
    ADD_PLUGIN_DIRECTORY(dae)
ENDIF()

IF(FBX_FOUND AND OSG_CPP_EXCEPTIONS_AVAILABLE)
    ADD_PLUGIN_DIRECTORY(fbx)
ENDIF()

IF(OPENCASCADE_FOUND)
    ADD_PLUGIN_DIRECTORY(OpenCASCADE)
ENDIF()

ADD_PLUGIN_DIRECTORY(bvh)
ADD_PLUGIN_DIRECTORY(x)
ADD_PLUGIN_DIRECTORY(dxf)
ADD_PLUGIN_DIRECTORY(OpenFlight)

ADD_PLUGIN_DIRECTORY(obj)

ADD_PLUGIN_DIRECTORY(pic)
ADD_PLUGIN_DIRECTORY(stl)
IF(NOT ANDROID)
    ADD_PLUGIN_DIRECTORY(3ds)
ENDIF()
ADD_PLUGIN_DIRECTORY(ac)
ADD_PLUGIN_DIRECTORY(pov)

ADD_PLUGIN_DIRECTORY(logo)
ADD_PLUGIN_DIRECTORY(lws)
ADD_PLUGIN_DIRECTORY(md2)
ADD_PLUGIN_DIRECTORY(osgtgz)
ADD_PLUGIN_DIRECTORY(tgz)

ADD_PLUGIN_DIRECTORY(shp)
ADD_PLUGIN_DIRECTORY(txf)

ADD_PLUGIN_DIRECTORY(bsp)
ADD_PLUGIN_DIRECTORY(mdl)

ADD_PLUGIN_DIRECTORY(gles)
ADD_PLUGIN_DIRECTORY(osgjs)

IF(OSG_CPP_EXCEPTIONS_AVAILABLE)
    ADD_PLUGIN_DIRECTORY(lwo)
    ADD_PLUGIN_DIRECTORY(ply)
    ADD_PLUGIN_DIRECTORY(txp)
ENDIF()

IF(FFMPEG_FOUND AND OSG_CPP_EXCEPTIONS_AVAILABLE)
    ADD_PLUGIN_DIRECTORY(ffmpeg)
ENDIF()

IF(GSTREAMER_FOUND AND GLIB_FOUND)
    ADD_PLUGIN_DIRECTORY(gstreamer)
ENDIF()

IF(DIRECTSHOW_FOUND)
    ADD_PLUGIN_DIRECTORY(directshow)
ENDIF()

# IF(OPENAL_FOUND)
#    ADD_SUBDIRECTORY(OpenAL)
# ENDIF()

IF(APPLE AND NOT ANDROID)
    ADD_PLUGIN_DIRECTORY(imageio)
ENDIF()

IF(AV_FOUNDATION_FOUND)
    ADD_PLUGIN_DIRECTORY(avfoundation)
ENDIF()

IF(QUICKTIME_FOUND)
    ADD_PLUGIN_DIRECTORY(quicktime)
ENDIF()

IF(QTKIT_FOUND)
    ADD_PLUGIN_DIRECTORY(QTKit)
ENDIF()

IF(NVTT_FOUND)
    ADD_PLUGIN_DIRECTORY(nvtt)
ENDIF()


IF(FREETYPE_FOUND)
    ADD_PLUGIN_DIRECTORY(freetype)
ENDIF()

IF(ZLIB_FOUND)
    ADD_PLUGIN_DIRECTORY(zip)
ENDIF()

IF(RSVG_FOUND)
    ADD_PLUGIN_DIRECTORY(svg)
ENDIF()

IF(POPPLER_FOUND)
    ADD_PLUGIN_DIRECTORY(pdf)
ENDIF()

IF(LIBVNCSERVER_FOUND)
    ADD_PLUGIN_DIRECTORY(vnc)
ENDIF()

ADD_PLUGIN_DIRECTORY(pvr)

ADD_PLUGIN_DIRECTORY(osc)
ADD_PLUGIN_DIRECTORY(trk)
ADD_PLUGIN_DIRECTORY(tf)

IF(LIBLAS_FOUND)
    ADD_PLUGIN_DIRECTORY(las)
ENDIF()


####################################################
#
# ScriptEngines
#
IF ((LUA_LIBRARIES AND LUA_INCLUDE_DIR) OR (LUA_SOURCE_DIR AND LUA_INCLUDE_DIR) OR (OSG_USE_LOCAL_LUA_SOURCE))
    ADD_PLUGIN_DIRECTORY(lua)
ENDIF()


# V8 and Python plugins are tests for linking against these libraries but aren't functionality beyond this.
# IF (V8_FOUND)
#     ADD_PLUGIN_DIRECTORY(V8)
# ENDIF()
#
# IF (PYTHONLIBS_FOUND)
#     ADD_PLUGIN_DIRECTORY(python)
# ENDIF()

####################################################
#
# Device integration plugins
#
IF   (SDL_FOUND)
    ADD_PLUGIN_DIRECTORY(sdl)
ENDIF(SDL_FOUND)

IF(ASIO_FOUND)
    ADD_PLUGIN_DIRECTORY(RestHttpDevice)
ENDIF(ASIO_FOUND)


IF(ZEROCONF_FOUND)
    ADD_PLUGIN_DIRECTORY(ZeroConfDevice)
ENDIF()

##########to get all the variables of Cmake
#GET_CMAKE_PROPERTY(MYVARS VARIABLES)
#FOREACH(myvar ${MYVARS})
#    FILE(APPEND ${CMAKE_CURRENT_BINARY_DIR}/AllVariables.txt
#        "${myvar} -->${${myvar}}<-\n"
#    )
#ENDFOREACH(myvar)


