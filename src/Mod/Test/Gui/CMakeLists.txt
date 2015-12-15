include_directories(
    ${CMAKE_CURRENT_BINARY_DIR}
    ${CMAKE_SOURCE_DIR}/src
    ${QT_INCLUDE_DIR}
    ${PYTHON_INCLUDE_DIRS}
    ${XercesC_INCLUDE_DIRS}
)

set(TestGui_LIBS
    FreeCADGui
)

set(Test_MOC_HDRS
    UnitTestImp.h
)
if(FREECAD_USE_QT5)
    fc_qt5_wrap_cpp(Test_MOC_SRCS ${Test_MOC_HDRS})
else(FREECAD_USE_QT5)
    fc_wrap_cpp(Test_MOC_SRCS ${Test_MOC_HDRS})
endif(FREECAD_USE_QT5)

SOURCE_GROUP("Moc" FILES ${Test_MOC_SRCS})

set(Dialogs_UIC_SRCS
    UnitTest.ui
)

if(FREECAD_USE_QT5)
    qt5_wrap_ui(Dialogs_UIC_HDRS ${Dialogs_UIC_SRCS})
else(FREECAD_USE_QT5)
    qt4_wrap_ui(Dialogs_UIC_HDRS ${Dialogs_UIC_SRCS})
endif(FREECAD_USE_QT5)

SET(Dialogs_SRCS
    ${Dialogs_UIC_HDRS}
    UnitTest.ui
    UnitTestImp.cpp
    UnitTestImp.h
    UnitTestPy.cpp
    UnitTestPy.h
)
SOURCE_GROUP("Dialogs" FILES ${Dialogs_SRCS})

if(FREECAD_USE_QT5)
    qt5_add_resources(Resource_SRCS Resources/Test.qrc)
else(FREECAD_USE_QT5)
    qt4_add_resources(Resource_SRCS Resources/Test.qrc)
endif(FREECAD_USE_QT5)

SET(Resource_SRCS
    ${Resource_SRCS}
    Resources/Test.qrc
)
SOURCE_GROUP("Resource" FILES ${Resource_SRCS})

SET(TestGui_SRCS
    ${Dialogs_SRCS}
    ${Resource_SRCS}
    AppTestGui.cpp
    PreCompiled.cpp
    PreCompiled.h
)
SET(TestGuiPy_SRCS
    qtunittest.py
)

add_library(QtUnitGui SHARED ${TestGui_SRCS})

target_link_libraries(QtUnitGui ${TestGui_LIBS})
add_dependencies(QtUnitGui Test)

fc_copy_sources(QtUnitGui "${CMAKE_BINARY_DIR}/Mod/Test" qtunittest.py)

SET_BIN_DIR(QtUnitGui QtUnitGui /Mod/Test)
SET_PYTHON_PREFIX_SUFFIX(QtUnitGui)

INSTALL(TARGETS QtUnitGui DESTINATION ${CMAKE_INSTALL_LIBDIR})
INSTALL(FILES ${TestGuiPy_SRCS} DESTINATION Mod/Test)

