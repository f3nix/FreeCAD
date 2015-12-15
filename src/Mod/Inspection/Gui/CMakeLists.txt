include_directories(
    ${CMAKE_SOURCE_DIR}/src
    ${CMAKE_CURRENT_BINARY_DIR}
    ${Boost_INCLUDE_DIRS}
    ${COIN3D_INCLUDE_DIRS}
    ${QT_INCLUDE_DIR}
    ${PYTHON_INCLUDE_DIRS}
    ${XercesC_INCLUDE_DIRS}
    ${ZLIB_INCLUDE_DIR}
)

link_directories(${OCC_LIBRARY_DIR})

set(InspectionGui_LIBS
    Inspection
    FreeCADGui
)

if(FREECAD_USE_QT5)
    qt5_add_resources(Inspection_QRC_SRCS Resources/Inspection.qrc)
else(FREECAD_USE_QT5)
    qt4_add_resources(Inspection_QRC_SRCS Resources/Inspection.qrc)
endif(FREECAD_USE_QT5)

set(InspectionGui_MOC_HDRS
    VisualInspection.h
)
if(FREECAD_USE_QT5)
    fc_qt5_wrap_cpp(InspectionGui_MOC_SRCS ${InspectionGui_MOC_HDRS})
else(FREECAD_USE_QT5)
    fc_wrap_cpp(InspectionGui_MOC_SRCS ${InspectionGui_MOC_HDRS})
endif(FREECAD_USE_QT5)

SOURCE_GROUP("Moc" FILES ${InspectionGui_MOC_SRCS})

set(Dialogs_UIC_SRCS
    VisualInspection.ui
)
if(FREECAD_USE_QT5)
    qt5_wrap_ui(Dialogs_UIC_HDRS ${Dialogs_UIC_SRCS})
else(FREECAD_USE_QT5)
    qt4_wrap_ui(Dialogs_UIC_HDRS ${Dialogs_UIC_SRCS})
endif(FREECAD_USE_QT5)

SET(Dialogs_SRCS
    ${Dialogs_UIC_HDRS}
    VisualInspection.cpp
    VisualInspection.h
)
SOURCE_GROUP("Dialogs" FILES ${Dialogs_SRCS})

SET(InspectionGui_SRCS
    ${Inspection_QRC_SRCS}
    ${Dialogs_SRCS}
    AppInspectionGui.cpp
    Command.cpp
    PreCompiled.cpp
    PreCompiled.h
    ViewProviderInspection.cpp
    ViewProviderInspection.h
    Workbench.cpp
    Workbench.h
)

add_library(InspectionGui SHARED ${InspectionGui_SRCS})
target_link_libraries(InspectionGui ${InspectionGui_LIBS})


fc_target_copy_resource(InspectionGui 
    ${CMAKE_SOURCE_DIR}/src/Mod/Inspection
    ${CMAKE_BINARY_DIR}/Mod/Inspection
    InitGui.py)

SET_BIN_DIR(InspectionGui InspectionGui /Mod/Inspection)
SET_PYTHON_PREFIX_SUFFIX(InspectionGui)

INSTALL(TARGETS InspectionGui DESTINATION ${CMAKE_INSTALL_LIBDIR})
