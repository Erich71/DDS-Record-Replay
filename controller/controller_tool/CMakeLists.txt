# Copyright 2023 Proyectos y Sistemas de Mantenimiento SL (eProsima).
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

###############################################################################
# CMake build rules for Submodule
###############################################################################
cmake_minimum_required(VERSION 3.5)

# Done this to set machine architecture and be able to call cmake_utils
enable_language(CXX)

###############################################################################
# Find package cmake_utils
###############################################################################
# Package cmake_utils is required to get every cmake macro needed
find_package(cmake_utils REQUIRED)

###############################################################################
# Project
###############################################################################
# Configure project by info set in project_settings.cmake
# - Load project_settings variables
# - Read version
# - Set installation paths
configure_project()

# Call explictly project
project(
    ${MODULE_NAME}
    VERSION
        ${MODULE_VERSION}
    DESCRIPTION
        ${MODULE_DESCRIPTION}
)

option(BUILD_DDSRECORDER_CONTROLLER "Whether to build or not the Controller tool" OFF)
if (BUILD_DDSRECORDER_CONTROLLER)

    message(STATUS "Building DDS Recorder controller tool")

    ###############################################################################
    # Build and install types
    ###############################################################################

    # Add subdirectory based on Fast DDS version
    add_subdirectory(ddstypes/DdsRecorderCommand)
    add_subdirectory(ddstypes/DdsRecorderStatus)

    ###############################################################################
    # Install Tool
    ###############################################################################
    add_subdirectory(tool)

    ###############################################################################
    # Packaging
    ###############################################################################
    # Install package
    eprosima_packaging()

else()
    message(STATUS "Not building DDS Recorder controller tool")
endif()
