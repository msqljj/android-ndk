#
# Copyright (C) The Android Open Source Project
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

cmake_minimum_required(VERSION 3.4.1)

# build native_app_glue as a static lib
add_library(nativeAppGlue STATIC
    ${ANDROID_NDK}/sources/android/native_app_glue/android_native_app_glue.c)

# build the ndk-helper library
set(ndkHelperSrc ../../../../common/ndk_helper)
add_subdirectory(${ndkHelperSrc} ndkHelperBin)

# now build app's shared lib
add_library(MoreTeapotsNativeActivity
  SHARED
    MoreTeapotsNativeActivity.cpp
    MoreTeapotsRenderer.cpp
)
set_target_properties(MoreTeapotsNativeActivity
  PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED YES
    CXX_EXTENSIONS NO
)
target_compile_features(MoreTeapotsNativeActivity
  PRIVATE cxx_noexcept
)
target_compile_options(MoreTeapotsNativeActivity
  PRIVATE
    -Wall -Werror -fno-rtti
)
# Export ANativeActivity_onCreate(),
# Refer to: https://github.com/android-ndk/ndk/issues/381.
set(CMAKE_SHARED_LINKER_FLAGS
    "${CMAKE_SHARED_LINKER_FLAGS} -u ANativeActivity_onCreate")

target_include_directories(MoreTeapotsNativeActivity
  PRIVATE
    ${ANDROID_NDK}/sources/android/cpufeatures
    ${ANDROID_NDK}/sources/android/native_app_glue
    ${ndkHelperSrc})

# add lib dependencies
target_link_libraries(MoreTeapotsNativeActivity
  PRIVATE
    android
    log
    atomic
    EGL
    GLESv2
    nativeAppGlue
    ndkHelper
)
