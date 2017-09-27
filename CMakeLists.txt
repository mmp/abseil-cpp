
cmake_minimum_required ( VERSION 3.0 )

project(abseil)

if(NOT IS_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/cctz")
  message(FATAL_ERROR "The cctz submodule directory is missing! "
    "You probably did not clone the project with --recursive, or first checked out "
    "abseil-cpp before it was added. Recover by running "
    "\"git submodule update --init --recursive\"")
endif()

add_subdirectory(cctz)

include_directories(
   .
   cctz/include
)

add_library(absl
    absl/base/dynamic_annotations.cc
    absl/base/internal/cycleclock.cc
    absl/base/internal/log_severity.cc
    absl/base/internal/low_level_alloc.cc
    absl/base/internal/malloc_extension.cc
    absl/base/internal/malloc_hook.cc
    absl/base/internal/raw_logging.cc
    absl/base/internal/spinlock.cc
    absl/base/internal/spinlock_wait.cc
    absl/base/internal/sysinfo.cc
    absl/base/internal/thread_identity.cc
    absl/base/internal/throw_delegate.cc
    absl/base/internal/unscaledcycleclock.cc
    absl/container/internal/test_instance_tracker.cc
    absl/debugging/internal/address_is_readable.cc
    absl/debugging/internal/elf_mem_image.cc
    absl/debugging/internal/vdso_support.cc
    absl/debugging/leak_check.cc
    absl/debugging/stacktrace.cc
    absl/numeric/int128.cc
    absl/strings/ascii.cc
    absl/strings/escaping.cc
    absl/strings/internal/memutil.cc
    absl/strings/internal/utf8.cc
    absl/strings/match.cc
    absl/strings/numbers.cc
    absl/strings/str_cat.cc
    absl/strings/str_replace.cc
    absl/strings/str_split.cc
    absl/strings/string_view.cc
    #absl/strings/strip.cc
    absl/strings/substitute.cc
    absl/synchronization/barrier.cc
    absl/synchronization/blocking_counter.cc
    absl/synchronization/internal/create_thread_identity.cc
    absl/synchronization/internal/graphcycles.cc
    #absl/synchronization/internal/mutex_nonprod.cc
    absl/synchronization/internal/per_thread_sem.cc
    absl/synchronization/internal/waiter.cc
    absl/synchronization/mutex.cc
    absl/synchronization/notification.cc
    absl/time/clock.cc
    absl/time/duration.cc
    absl/time/format.cc
    absl/time/internal/test_util.cc
    absl/time/time.cc
    absl/types/bad_any_cast.cc
    absl/types/bad_optional_access.cc
    absl/types/optional.cc
    absl/utility/utility.cc
)

target_compile_features ( absl PRIVATE
    #cxx_std_11
    cxx_alignas
    cxx_attributes
    cxx_constexpr
    cxx_defaulted_functions
    cxx_noexcept
    cxx_override
    cxx_right_angle_brackets
    cxx_strong_enums
    cxx_variadic_templates
)

###########################################################################
# tests

if(NOT IS_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/googletest")
  message(FATAL_ERROR "The googletest submodule directory is missing! "
    "You probably did not clone the project with --recursive, or first checked out "
    "abseil-cpp before it was added. Recover by running "
    "\"git submodule update --init --recursive\"")
endif()

add_subdirectory(googletest)

enable_testing()

include_directories(${gtest_SOURCE_DIR}/include ${gtest_SOURCE_DIR})
include_directories(${gmock_SOURCE_DIR}/include ${gmock_SOURCE_DIR})

add_definitions(-D ABSL_MALLOC_EXTENSION_TEST_ALLOW_MISSING_EXTENSION)

add_executable(absl_test
    absl/algorithm/algorithm_test.cc
    absl/algorithm/container_test.cc
    absl/base/bit_cast_test.cc
    absl/base/call_once_test.cc
    absl/base/config_test.cc
    absl/base/internal/endian_test.cc
    absl/base/internal/low_level_alloc_test.cc
    absl/base/internal/malloc_extension_test.cc
    absl/base/internal/sysinfo_test.cc
    absl/base/internal/thread_identity_test.cc
    absl/base/invoke_test.cc
    absl/base/raw_logging_test.cc
    absl/base/throw_delegate_test.cc
    absl/container/fixed_array_test.cc
    absl/container/inlined_vector_test.cc
    absl/container/internal/test_instance_tracker_test.cc
    absl/debugging/leak_check_test.cc
    absl/memory/memory_test.cc
    absl/meta/type_traits_test.cc
    absl/numeric/int128_test.cc
    absl/strings/ascii_test.cc
    absl/strings/escaping_test.cc
    absl/strings/internal/char_map_test.cc
    absl/strings/internal/memutil_test.cc
    absl/strings/internal/ostringstream_test.cc
    absl/strings/internal/resize_uninitialized_test.cc
    absl/strings/internal/utf8_test.cc
    absl/strings/match_test.cc
    absl/strings/numbers_test.cc
    absl/strings/str_cat_test.cc
    absl/strings/str_join_test.cc
    absl/strings/str_replace_test.cc
    absl/strings/str_split_test.cc
    absl/strings/string_view_test.cc
    absl/strings/strip_test.cc
    absl/strings/substitute_test.cc
    absl/synchronization/barrier_test.cc
    absl/synchronization/blocking_counter_test.cc
    absl/synchronization/internal/graphcycles_test.cc
    absl/synchronization/internal/per_thread_sem_test.cc
    absl/synchronization/mutex_test.cc
    absl/synchronization/notification_test.cc
    absl/time/clock_test.cc
    absl/time/duration_test.cc
    absl/time/format_test.cc
    absl/time/time_norm_test.cc
    absl/time/time_test.cc
    absl/time/time_zone_test.cc
    absl/types/any_test.cc
    absl/types/optional_test.cc
    absl/types/span_test.cc
    absl/utility/utility_test.cc
)

target_compile_features ( absl_test PRIVATE
    #cxx_std_11
    cxx_attributes
    cxx_constexpr
    cxx_decltype
    cxx_right_angle_brackets
    cxx_rvalue_references
    cxx_strong_enums
)

target_link_libraries(absl_test absl cctz gtest_main gmock)

add_test(test absl_test)
