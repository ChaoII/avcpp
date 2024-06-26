cmake_minimum_required (VERSION 3.19)

# transcoder sources

if(NOT DEFINED PROJECT_NAME)
    set(AVCPP_SAMPLES_NOT_SUBPROJECT ON)
endif()

project(AvCppSamples LANGUAGES CXX VERSION 2.3.2)

if(AVCPP_SAMPLES_NOT_SUBPROJECT)
    find_package(avcpp REQUIRED)
endif()

set(TARGETS
  api2-decode
  api2-decode-encode-video
  api2-scale-video
  api2-decode-audio
  api2-decode-rasample-audio
  api2-decode-encode-audio
  api2-dict-basic
  api2-timestamp
  api2-demux-seek
  api2-remux
  api2-hw-encode
)

foreach(target ${TARGETS})
    add_executable(${target} ${target}.cpp)

    target_link_libraries(${target}
        avcpp::avcpp
    )

    if (AVCPP_WARNING_OPTIONS)
        target_compile_options(${target} PRIVATE ${AVCPP_WARNING_OPTIONS})
    endif()

    if(WIN32)
        target_link_libraries(${target} ws2_32)
    endif()

endforeach()
