INCLUDE_DIRECTORIES(
     .
     gui
     ${CMAKE_BINARY_DIR}/plugins/NavStars/src
     ${CMAKE_BINARY_DIR}/plugins/NavStars/src/gui
)

LINK_DIRECTORIES(${CMAKE_BINARY_DIR}/src)

SET(NavStars_SRCS
     NavStars.hpp
     NavStars.cpp
     NavStarsCalculator.hpp
     NavStarsCalculator.cpp
     gui/NavStarsWindow.hpp
     gui/NavStarsWindow.cpp
)

SET(NavStars_UIS
     gui/navStarsWindow.ui
)

SET(NavStars_RES ../resources/NavStars.qrc)
IF (${QT_VERSION_MAJOR} EQUAL "5")
     QT5_WRAP_UI(NavStars_UIS_H ${NavStars_UIS})
     QT5_ADD_RESOURCES(NavStars_RES_CXX ${NavStars_RES})
ELSE()
     QT_WRAP_UI(NavStars_UIS_H ${NavStars_UIS})
     QT_ADD_RESOURCES(NavStars_RES_CXX ${NavStars_RES})
ENDIF()

ADD_LIBRARY(NavStars-static STATIC ${NavStars_SRCS} ${NavStars_RES_CXX} ${NavStars_UIS_H})
TARGET_LINK_LIBRARIES(NavStars-static Qt${QT_VERSION_MAJOR}::Core Qt${QT_VERSION_MAJOR}::Widgets)

IF(ENABLE_TESTING)
    add_definitions("-DWITH_TESTING_ON")
    ADD_SUBDIRECTORY(test)
ENDIF(ENABLE_TESTING)

SET_TARGET_PROPERTIES(NavStars-static PROPERTIES OUTPUT_NAME "NavStars")
SET_TARGET_PROPERTIES(NavStars-static PROPERTIES COMPILE_FLAGS "-DQT_STATICPLUGIN")
ADD_DEPENDENCIES(AllStaticPlugins NavStars-static)

SET_TARGET_PROPERTIES(NavStars-static PROPERTIES FOLDER "plugins/NavStars")
