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

## add_library

```cmake
add_library(<name> <SHARED|STATIC|MODULE|UNKNOWN> IMPORTED
            [GLOBAL])
```

- 添加一个名为name的lib到工程中，并指定这个lib的源文件

## target_include_directories

```cmake
target_include_directories(<target> [SYSTEM] [BEFORE]
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])
```

- 指定编译目标文件target的时候需要包含的路径和内容

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

