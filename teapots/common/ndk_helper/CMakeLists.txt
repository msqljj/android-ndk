
add_library(ndkHelper
  STATIC
    gestureDetector.cpp
    gl3stub.cpp
    GLContext.cpp
    interpolator.cpp
    JNIHelper.cpp
    perfMonitor.cpp
    sensorManager.cpp
    shader.cpp
    tapCamera.cpp
    vecmath.cpp
)

set_target_properties(ndkHelper
  PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED YES
    CXX_EXTENSIONS NO
)
target_compile_features(ndkHelper
  PUBLIC
    cxx_noexcept
    cxx_nullptr
)
target_compile_options(ndkHelper
  PRIVATE
    -Wall -Werror -fno-rtti
)

target_include_directories(ndkHelper
  PRIVATE
    ${ANDROID_NDK}/sources/android/native_app_glue
)
