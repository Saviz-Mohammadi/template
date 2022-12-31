# Project properties
# ========================================================================================================================================


# MINIMUM_REQUIRED
#
# CMake is an ever changing language that becomes consistantly more effecient and powerful as time goes by.
# This command is improtant as it specifies what capabilities and changes this version brings to the table.
cmake_minimum_required(VERSION 3.5)

# PROJECT
#
# Project is an improtant command. It is used to set the overall properties of the project.
project(

    # The name that you specify here, will be stored in a global predefined variable named "${PROJECT_NAME}".
    # It can be used throughout the entire file to refer to this project.
    ProjectName

    # Project version
    VERSION 0.1

    # Project programming language (CXX means C++)
    LANGUAGES CXX
)


# ========================================================================================================================================





# Important variables
# ========================================================================================================================================


# Here is a list of important predefined variables in the CMake system:

# In general every variable that inculdes the name "SOURCE" in its definition, has a corresponding variable with the name "BINARY".
# These variables preform mainly the same functionality with the difference that the one the name "SOURCE" refers to the source files of the project.
# whereas the one with the name "BINARY" refers to the actual executable created after compilation is complete.

# ${CMAKE_SOURCE_DIR} >> Contains the path to the top level source tree of the current project. (The root folder of all of our source files)
# ${CMAKE_BINARY_DIR} >> Contains the path to the top level binary tree of the current project. (The root folder of all of our binary files)

# Important note about the above two variables:
#
# These two variables will never change once they are initialized!
# ${CMAKE_SOURCE_DIR} will alawys refer to the directory in which the top most level "CMakeLists.txt" file was found.
# ${CMAKE_BINARY_DIR} will alawys refer to the directory in which the build file was found.

message(STATUS "The CMAKE_SOURCE_DIR path is: ${CMAKE_SOURCE_DIR}\n")
message(STATUS "The CMAKE_BINARY_DIR path is: ${CMAKE_BINARY_DIR}\n")

# All other commands below will be dependent from which "CMakeLists.txt" file they will get called.

# ${CMAKE_CURRENT_SOURCE_DIR} >> Contains the path to source directory currently being processed. (The parent folder of the current CMake file)
# ${CMAKE_CURRENT_BINARY_DIR} >> Contains the path to binary directory currently being processed. (The parent folder of the binary file of the process)

# ${PROJECT_NAME} >> Contains the name given to the most recent "project()" command. (The name of our project)
# ${PROJECT_VERSION} >> Contains the version given to the most recent "project()" command. (The version of our project)


# ========================================================================================================================================





# Compiler options
# ========================================================================================================================================


# CMAKE_INCLUDE_CURRENT_DIR
#
# This command is often used in Qt projects.
# It will automatically include the binary and source directories of the project. (Really useful for MOC)
# This command is the same as having the following command:
#
# INCLUDE_DIRECTORIES(
#       ${CMAKE_CURRENT_BINARY_DIR}
#       ${CMAKE_CURRENT_SOURCE_DIR}
# )
set(CMAKE_INCLUDE_CURRENT_DIR ON)


# CMAKE_AUTOUIC
#
# This command will call UIC (User Interface Compiler) automatically when needed. (Converts ".ui" files to "ui_*.h" files)
set(CMAKE_AUTOUIC ON)


# CMAKE_AUTOMOC
#
# This command will call MOC (Meta Object Compiler) automatically when needed. (Establishes the core functionalities of QObject. Like signals-slots)
set(CMAKE_AUTOMOC ON)


# CMAKE_AUTORCC
#
# This command will call RCC (Resource Complier) automatically when needed. (Handles resource files found in ".qrc" file)
set(CMAKE_AUTORCC ON)


# CMAKE_CXX_STANDARD
#
# This command specifies the standard version of C++ that will be used in this project (C++17)
set(CMAKE_CXX_STANDARD 17)


# CMAKE_CXX_STANDARD_REQUIRED
#
# This command specifies that the version set by the "CMAKE_CXX_STANDARD" command (seen above) is a requirement for this project to work.
# It will cause an error if it detects the version installed/used on the device is lower than the requirment.
#
# Description:
# C++ like any other language changes with each verison and update. Therfore, it is important to specify exactly which version is needed.
# This is since each version presents its own capabilities and syntax.
set(CMAKE_CXX_STANDARD_REQUIRED ON)


# ========================================================================================================================================





# Packages
# ========================================================================================================================================


# FIND_PACKAGE
#
# This command finds and adds the libraries specified to our project.
# In this case, we are finding the Qt6/5 libraries that are required for Widgets to work.
find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets)


# ========================================================================================================================================





# Files
# ========================================================================================================================================


# There are two ways to approach in finding/adding ".h" (header) and ".cpp" (implementation/source) files:

# 1- Manually specifie the path of each file in a variable and add them to the executable:
#
# This method is a little tidious as you have to specifie the path to each of the ".h" (header) and ".cpp" (implementation/source) files
# by adding them to their respective variables. "PROJECT_HEADERS" for headers files and "PROJECT_SOURCES" for implementation files.

# Note: when specifing the path for the files you must use the full path

#set(HEADER_FILES
#
#)

#set(SOURCE_FILES
#
#)

#set(UI_FILES
#
#)

#set(QRC_FILES
#
#)



# 2- The second method involves using the following commands:


# FILE
#
# This command will attempt to find and add every file with the extention of ".cpp".
# We are storing the found files in the custom variable called "SOURCE_FILES".
# The attribute "GLOB_RECURSE" specifies to recursively search the subdirectiories of the "libraries" folder.
file(GLOB_RECURSE SOURCE_FILES "libraries/*.cpp")


# We attempt to do the same for header files by searching for ".h" extention.
# We store them in the variable called "HEADER_FILES".
file(GLOB_RECURSE HEADER_FILES "libraries/*.h")


# We attempt to do the same for header files by searching for ".ui" extention.
# We store them in the variable called "UI_FILES".
file(GLOB_RECURSE UI_FILES "libraries/*.ui")


# We attempt to do the same for header files by searching for ".qrc" extention.
# We store them in the variable called "QRC_FILES".
file(GLOB_RECURSE QRC_FILES "libraries/*.qrc")



# After either method 1 or method 2, one must make sure to add every variable to your executable:

# add_executable(
#
#       ${PROJECT_NAME}
#       ${HEADER_FILES}
#       ${SOURCE_FILES}
#       ${UI_FILES}
#       ${QRC_FILES}
# )

# Normally, we do not need to include header files, but since the MOC in Qt requires all files to be declared,
# it is mandatory to have them.


# ========================================================================================================================================





# Executable
# ========================================================================================================================================


# To create an executable, we must make sure to feed the following commands with all the files that are required.
# The if-statements are used to identify weather we are using Qt6 version and above. Or not.
# Depending on the version we determine to do additional processes. (Such as "Finalization")

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)

    # QT_ADD_EXECUTABLE
    #
    # Very similar to "add_executable" command.
    # The main difference between "qt_add_executable" and "add_executable" is the following:
    #
    # "qt_add_executable" provides additional logic to automatically handle processes specific to Qt.
    # This involves things such as: The linking of Qt plugins in static Qt builds, finilization checks, etc.
    qt_add_executable(

        # Executable file name
        ${PROJECT_NAME}

        # Specifies that "Finalization" process will be called manually.
        MANUAL_FINALIZATION

        # Additional files needed
        main.cpp
        ${HEADER_FILES}
        ${SOURCE_FILES}
        ${UI_FILES}
        ${QRC_FILES}
    )

else()

    # ADD_EXECUTABLE
    #
    # This command creates an executbale for our project.
    add_executable(

        # Executable file name
        ${PROJECT_NAME}

        # Additional files needed
        main.cpp
        ${HEADER_FILES}
        ${SOURCE_FILES}
        ${UI_FILES}
        ${QRC_FILES}
    )

endif()


# ========================================================================================================================================





# Header file tricks
# ========================================================================================================================================


# The "file(GLOB_RECURSE INCLUDE_FILES *.h)" command Adds headers to the list of files to the executable.
# Unfortunately, this doesn't actually add them to the compiler's search path.It only adds them to the project's folder structure in IDEs.
# To properly add header files use the command below.

# TARGET_INCLUDE_DIRECTORIES
#
# This command makes the paths of the "#include" preprocessors shorter by giving the compiler a hint as to where to look for header files.
# Under normal circumstances this would be the ideal way to add the header files to both project and executabl.
# However, one still needs to include the header files in the executable in order for the MOC to work and your project to build.

target_include_directories(

    # Target
    ${PROJECT_NAME}

    # Access specifier (More important in terms of building libraries than executable)
    PUBLIC

    # Path to the root of header files (You can include all of them, but...)
    "${CMAKE_CURRENT_LIST_DIR}/Libraries/Header Files"

)


# ========================================================================================================================================





# Adding librarires needed for executable
# ========================================================================================================================================


# TARGET_LINK_LIBRARIES
#
# In order for the executable to work properly, it still requires the external libraries and files.
# Therfore, we need to specify the libraries and link them to the executable.
target_link_libraries(

    # Executable name (Used to identify which executable we are trying to link to)
    ${PROJECT_NAME}

    # Mode (More important in terms of building libraries than executable)
    PRIVATE

    # Libraries
    Qt${QT_VERSION_MAJOR}::Widgets
    Qt${QT_VERSION_MAJOR}::Gui
    Qt${QT_VERSION_MAJOR}::Core

    # The "${QT_VERSION_MAJOR}" argument can simply be neglected or replaced with an actual number like 6 for "Qt6".
    # The reason why we use this instead of an actual number is because it makes the compiling more flexible by refering
    # to the actual version of Qt that we need to fetch the libraries for.
)


# SET_TARGET_PROPERTIES
#
# We can define additional properties for our executable.
# Such as for what system we are trying to build for (Windows, Mac, ...).
set_target_properties(

    # Executable name
    ${PROJECT_NAME}

    # Properties
    PROPERTIES
    MACOSX_BUNDLE_GUI_IDENTIFIER my.example.com
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

# Not sure what this command does
install(
    TARGETS ${PROJECT_NAME}
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
)


# ========================================================================================================================================





# "Finalization" process
# ========================================================================================================================================


# The if-statements are used to identify weather we are using Qt6 version and above. Or not.
# Depending on the version we determine to do additional processes. (Such as "Finalization")
if(QT_VERSION_MAJOR EQUAL 6)

    # If Qt version is 6 or above, then call "Finalization" command.
    qt_finalize_executable(

        # Executbale file name
        ${PROJECT_NAME}
    )

endif()

    # If Qt version is less than 6, then do not call "Finalization" command. (Just do nothing)


# ========================================================================================================================================
