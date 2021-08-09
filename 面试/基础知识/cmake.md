## cmake_minimun_required(VERSION 3.8.2)

-   设置最低的cmake版本，如果低于这个版本，会报错

## project(project name)

-   set the project name, use it as ${PROJECT_NAME}

## add_executable

-   Add an executable to the project using the specified source files

    ```cmake
    add_executable(<name> [WIN32] [MACOSX_BUNDLE]
                   [EXCLUDE_FROM_ALL]
                   [source1] [source2 ...])
    add_executable(main.out main.cpp)
    ```

## find_package

- 用途：添加第三方依赖库

  ```cmake
  find_package(Boost 1.46.1 REQUIRED COMPONENTS filesystem system) 
  ```

  - Boost - Name of the library. This is part of used to find the module file FindBoost.cmake
  - 1.46.1 - The minimum version of boost to find
  - REQUIRED - Tells the module that this is required and to fail it it cannot be found
  - COMPONENTS - The list of libraries to find.

## add_library

```cmake
add_library(<name> <SHARED|STATIC|MODULE|UNKNOWN> IMPORTED
            [GLOBAL])
            
add_library(mylib STATIC source/show.cpp) #创建一个静态库
```

- 通过source file去创建一个library
-   `STATIC` libraries are archives of object files for use when linking other targets.
-  `SHARED` libraries are linked dynamically and loaded at runtime.  
- `MODULE` libraries are plugins that are not linked into other targets but may be loaded dynamically at runtime using dlopen-like functionality.

## target_include_directories

```cmake
target_include_directories(<target> [SYSTEM] [BEFORE]
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])
  
  target_include_directories(${PROJECT_NAME}  PUBLIC  ${PROJECT_SOURCE_DIR}/include)
```

- 指定编译目标文件target的时候需要包含的路径和内容

  ```cmake
  #在g++命令行中相当于
  g++ main.cpp show.cpp -o main -I ./include
  #目录结构是：include文件中包含了show.h头文件
  ```

  

- 经过尝试，发现这条指令必须在add_executable后面，因为add_executable后才生成target吗？

## target_link_libraries

```cmake
target_link_libraries(<target> [item1] [item2] [...]
                      [[debug|optimized|general] <item>] ...)
```

- 该指令的作用为将目标文件target与库文件进行链接

## add_subdirectory

```cmake
add_subdirectory (source_dir [binary_dir] [EXCLUDE_FROM_ALL])
```

- 添加一个子目录并构建该子目录

## 常用内置变量

-   注意：cmake**内置命令**是不区分大小写的，但是系统**内置变量**是区分大小写的！

|               variables               | info                                                         |
| :-----------------------------------: | ------------------------------------------------------------ |
| CMAKE_BINARY_DIR / PROJECT_BINARY_DIR | 指的是工程编译发生的目录，即如果是out of source编译，那么就是build目录 |
| CMAKE_SOURCE_DIR / PROJECT_SOURCE_DIR | 工程的顶层目录，里面有源文件，即root source directory        |
|       CMAKE_CURRENT_SOURCE_DIR        | 指的是当前处理的 CMakeLists.txt 所在的路径，即子工程所在的路径 |
|                                       |                                                              |

## 自设变量举例

-   自设变量是区分大小写的！换句话说，cmake里面的所有变量都区分大小写

```cmake
# Create a sources variable with a link to all cpp files to compile
set(SOURCES
    src/Hello.cpp
    src/main.cpp
)

add_executable(${PROJECT_NAME} ${SOURCES})
```

## BUILD_TYPE

```cmake
#命令行
-DCMAKE_BUILD_TYPE=Debug/Release/RelWithDebInfo/MinSizeRel
#CMakeLists.txt
SET(CMAKE_BUILD_TYPE "Debug")
```

## 其他

```cmake
#查看详细的debug信息
-VERBOSE=1

#make clean命令：用于清除make所产生的所有.o与可执行文件
```



## 常见问题

### cmake怎样分离源文件与头文件？

- 在add_executable中注意保证source地址正确，或者可以用set变量
- 利用target_include_directories

### c++中不是已经有了makefile了吗？为什么还需要cmake？

- 主要是为了跨平台。在linux/Unix平台下，生成makefile，在Windows下生成MSVC

### 什么是in source build ? 什么是out of source build? 有什么好处?

- out of source将生成的.o、binary、CMakeCache.txt与source文件分离，有利于rm -rf *清除build中的所有产生物，对source没有任何影响



