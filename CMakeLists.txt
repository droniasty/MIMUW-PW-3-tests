cmake_minimum_required (VERSION 3.11)
project (MIMUW-PW-lockfree C)

set(THREADS_PREFER_PTHREAD_FLAG TRUE)  # prefer -pthread to -lphtread
find_package (Threads REQUIRED)

set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_VERBOSE_MAKEFILE ON CACHE BOOL "ON" FORCE)

set(CMAKE_C_FLAGS "-Wall -Wextra -Wpedantic -Wno-sign-compare -Wno-unused-parameter -O3 -march=native -fno-exceptions")

# add_compile_options(-fsanitize=thread -Og -g)
# add_link_options(-fsanitize=thread -Og -g)

# add_compile_options(-fsanitize=address -Og -g)
# add_link_options(-fsanitize=address -Og -g)   twoqueues

add_library(queues OBJECT SimpleQueue.c RingsQueue.c LLQueue.c BLQueue.c HazardPointer.c)
target_link_libraries(queues PRIVATE Threads::Threads atomic)

add_executable(simpleTester simpleTester.c)
target_link_libraries(simpleTester PRIVATE queues Threads::Threads atomic)

add_executable(one_read_one_write one_read_one_write_tester.c)
target_link_libraries(one_read_one_write PRIVATE queues Threads::Threads atomic)

add_executable(one_read_one_write_tester_with_isempty one_read_one_write_tester_with_isempty.c)
target_link_libraries(one_read_one_write_tester_with_isempty PRIVATE queues Threads::Threads atomic)

add_executable(one_read_multiple_write one_read_multiple_write.c)
target_link_libraries(one_read_multiple_write PRIVATE queues Threads::Threads atomic)

add_executable(one_read_multiple_write_with_isempty one_read_multiple_write_with_isempty.c)
target_link_libraries(one_read_multiple_write_with_isempty PRIVATE queues Threads::Threads atomic)

add_executable(multiple_read_multiple_write multiple_read_multiple_write.c)
target_link_libraries(multiple_read_multiple_write PRIVATE queues Threads::Threads atomic)

add_executable(multiple_read_multiple_write_with_isempty multiple_read_multiple_write_with_isempty.c)
target_link_libraries(multiple_read_multiple_write_with_isempty PRIVATE queues Threads::Threads atomic)

add_executable(multiple_read_one_write multiple_read_one_write.c)
target_link_libraries(multiple_read_one_write PRIVATE queues Threads::Threads atomic)

add_executable(multiple_read_one_write_with_isempty multiple_read_one_write_with_isempty.c)
target_link_libraries(multiple_read_one_write_with_isempty PRIVATE queues Threads::Threads atomic)

