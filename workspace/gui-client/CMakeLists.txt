cmake_minimum_required(VERSION 3.16)

project(gui-client VERSION 0.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Qt6 6.4 REQUIRED COMPONENTS Quick QuickControls2)

qt_standard_project_setup()

qt_add_executable(appgui-client
    src/main.cpp

    include/app-backend/app-backend.hpp
    src/app-backend/app-backend.cpp
)

target_include_directories(appgui-client PUBLIC include/app-backend)
qt_add_qml_module(appgui-client
    URI app
    VERSION 1.0
    QML_FILES 
    qml/Main.qml

    qml/SignInPage.qml
    qml/SignUpPage.qml

    qml/widget/SignInWidget.qml
    qml/widget/SignUpWidget.qml
)

# Qt for iOS sets MACOSX_BUNDLE_GUI_IDENTIFIER automatically since Qt 6.1.
# If you are developing for iOS or macOS you should consider setting an
# explicit, fixed bundle identifier manually though.
set_target_properties(appgui-client PROPERTIES
#    MACOSX_BUNDLE_GUI_IDENTIFIER com.example.appgui-client
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

target_link_libraries(appgui-client
    PRIVATE Qt6::Quick Qt6::QuickControls2 proto
)

include(GNUInstallDirs)
install(TARGETS appgui-client
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)