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

