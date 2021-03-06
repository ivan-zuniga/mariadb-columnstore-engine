
INCLUDE_DIRECTORIES( ${ENGINE_COMMON_INCLUDES} )

ADD_CUSTOM_COMMAND(
    OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/dml-gram.cpp ${CMAKE_CURRENT_SOURCE_DIR}/dml-scan.cpp
    COMMAND ./dml-gram.sh ${BISON_EXECUTABLE}
    COMMAND ./dml-scan.sh ${LEX_EXECUTABLE}
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
    DEPENDS dml.y dml.l
)

ADD_CUSTOM_TARGET(dml-lexer DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/dml-scan.cpp)
ADD_CUSTOM_TARGET(dml-parser DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/dml-gram.cpp)

########### next target ###############

SET(dmlpackage_LIB_SRCS
    dml-scan.cpp
    dml-gram.cpp
    calpontdmlfactory.cpp
    calpontdmlpackage.cpp
    dmlcolumn.cpp
    deletedmlpackage.cpp
    dmlobject.cpp
    insertdmlpackage.cpp
    mysqldmlstatement.cpp
    oracledmlstatement.cpp
    row.cpp
    dmltable.cpp
    updatedmlpackage.cpp
    vendordmlstatement.cpp
    commanddmlpackage.cpp
    dmlpkg.cpp
    dmlparser.cpp)

ADD_LIBRARY(dmlpackage SHARED ${dmlpackage_LIB_SRCS})

ADD_DEPENDENCIES(dmlpackage dml-lexer dml-parser)

SET_TARGET_PROPERTIES(dmlpackage PROPERTIES VERSION 1.0.0 SOVERSION 1)

INSTALL(TARGETS dmlpackage DESTINATION ${ENGINE_LIBDIR} COMPONENT libs)

