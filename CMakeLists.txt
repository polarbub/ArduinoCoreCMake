# This file is the build system for the Arduino Core
# Copyright (C) 2024 polarbub

# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License version 3 as published by
# the Free Software Foundation.

# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.

# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Please note that you must set the include path for the correct pins_arduino.h, the avr-gcc compiler, and the compiler flags for your board

# Declare the minimum required CMake version
cmake_minimum_required(VERSION 3.28)

#Enable versioning
cmake_policy(SET CMP0048 NEW)

file(READ "ArduinoCore-avr/platform.txt" version)
string(REGEX MATCH "version=[0-9\\.]+" version "${version}")
string(REGEX REPLACE "version=([0-9\\.]+)" "\\1" version "${version}")

# set library metadata
project(arduino-core VERSION ${version} DESCRIPTION "Builds the arduino core with cmake" LANGUAGES CXX C ASM)

#Remove header from SRC_FILES regex:
#' src/[^\n ]*\.h'
# Define a variable containing a list of source files for the project

file(GLOB SRC_ARDUINO ArduinoCore-avr/cores/arduino/*.cpp ArduinoCore-avr/cores/arduino/*.c)

# Define the build target for the library
add_library(${PROJECT_NAME} ${SRC_ARDUINO})
set_target_properties(${PROJECT_NAME} PROPERTIES VERSION ${PROJECT_VERSION})

#target is used to only effect this library.
target_include_directories(${PROJECT_NAME} PUBLIC ArduinoCore-avr/cores/arduino)