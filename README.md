# check-interference-sizes-cmake
[![License](https://img.shields.io/badge/License-MIT--Clause-blue.svg)](https://github.com/yvoinov/check-interference-sizes-cmake/blob/main/LICENSE)
## CMake module to check C++ interference sizes

Unfortunately, many new features of the C ++ standard have not yet been implemented by compilers.

Moreover, compiler check macros often erroneously report the presence of a feature, but there is no implementation.

Based on [this module](https://github.com/yvoinov/check-interference-sizes) the similar solution was written for CMake.

To use, add include to CMakeLists.txt before targets as shown below (an example):

```sh
include(cmake_check_interference_sizes.cmake)
```

Module will check and set the variable INTERFERENCE_SIZES, which is uses for conditional compilation when determining the size of the cache line:

```c
#ifndef MY_CACHE_LINE_SIZE
#	if __cpp_lib_hardware_interference_size >= 201703 && defined INTERFERENCE_SIZES
#		define MY_CACHE_LINE_SIZE std::hardware_destructive_interference_size
#	else
#		define MY_CACHE_LINE_SIZE (((2 * sizeof(std::max_align_t)) & ((2 * sizeof(std::max_align_t)) - 1)) == 0 ? \
						(2 * sizeof(std::max_align_t)) : \
						nearestPowerof2(2 * sizeof(std::max_align_t)))
#	endif
#endif
```
