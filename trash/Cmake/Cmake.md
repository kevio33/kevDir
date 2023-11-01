# Cmake

**c++项目创建cmakelist文件**

```cmake
cmake_minimum_required(VERSION 3.0.0)
project(cpp_cygwin VERSION 0.1.0 LANGUAGES C CXX)

include(CTest)
enable_testing()
aux_source_directory(./10-25 SOURCES) #添加目录，将编译目录下的所有文件
add_executable(10-25  ${SOURCES}) #生成可执行文件10-25.exe

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
set(EXECUTABLE_OUTPUT_PATH ..) #生成的可执行文件位置
include(CPack)
```



**NDK项目cmakelist文件**

```cmake
#指定CMake的最小版本
cmake_minimum_required(VERSION 3.10.2)

#项目名字
project("ndkpro2")

include_directories("inc") #添加包含头文件的目录

file(GLOB allCPP *.c *.h *.cpp) #批量导入所有源文件

#创建一个静态或者动态库，并提供其关联的源文件路径，开发者可以定义多个库，CMake会自动去构建它们。Gradle可以自动将它们打包进APK中。
#第一个参数——native-lib：是库的名称，最后生成native-lib.so
#第二个参数——SHARED：是库的类别，是动态的还是静态的
#第三个参数——src/main/cpp/native-lib.cpp：是库的源文件的路径

add_library( # Sets the name of the library.
             native-lib

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             native-lib.cpp 
             
             ${allCPP} #上面导入的所有源文件
             )


#找到一个预编译的库，并作为一个变量保存起来。由于CMake在搜索库路径的时候会包含系统库，并且CMake会检查它自己之前编译的库的名字，所以开发者需要保证开发者自行添加的库的名字的独特性。
#第一个参数——log-lib：设置路径变量的名称
#第一个参数—— log：指定NDK库的名子，这样CMake就可以找到这个库

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )


#指定CMake链接到目标库。开发者可以链接多个库，比如开发者可以在此定义库的构建脚本，并且预编译第三方库或者系统库。
#第一个参数——native-lib：指定的目标库
#第一个参数——${log-lib}：将目标库链接到NDK中的日志库，

target_link_libraries( # Specifies the target library.
                       native-lib

                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib} )
```

