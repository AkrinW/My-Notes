# CMake Notes

Tags:  `CMake`

[TOC]

## 什么是CMake?

需要理解，对于大型的Cpp项目，在终端使用G++手动编译是相当痛苦的。
因此CMake被用来替代做这些事情，对于设定编译器参数，编译文件，链接库，
只需要设定一次就足够了，用一个CMake文件来替代每次重新写编译内容。
另外，CMake可以跨平台通用。

## 开始使用CMake

1. 非常简单，首先电脑需要安装CMake，并且在VSCode中安装CMake的扩展。

2. 在项目文件夹下新建一个CMakeLists.txt，在里面填写编译配置

    ```Makefile
        # a cmake list file
        cmake_minimum_required(VERSION 3.5)
        # project name
        project(CPPproject)

        # 定义变量
        SET(MAIN test.cpp)

        # 指定cpp版本
        SET(CMAKE_CXX_STANDARD 17)

        # 指定存储路径
        SET(Home "D:/study/code")
        SET(EXECUTABLE_OUTPUT_PATH ${Home})
        # 包含头文件
        include_directories("${PROJECT_SOURCE_DIR}/include")
        # 搜索当前目录的src目录下所有的源文件，并存储到变量中
        # 使用GLOB是不好的语法，所以注释了
        # file(GLOB MAIN_SRC "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp")
        # file(GLOB MAIN_HEAD "${CMAKE_CURRENT_SOURCE_DIR}/include/*.h")
        aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/src MAIN_SRC)
        include_directories(${PROJECT_SOURCE_DIR}/include)

        add_executable(test ${MAIN} ${MAIN_SRC})
    ```

3. 写完CMakeLists.txt后，按下`ctrl+shift+P`，打开VScode控制指令，找到`CMake: Configuration`，进行配置。配置完后，目录下会多出一个`./build`文件夹，里面就是CMake配置相关的文件了，可以把它们放进`.gitignore`里。

4. 在CPP项目的main文件里，按`F7`就可以进行编译了。

## CMake遇到的问题

需要注意CMake的语法，因为是全平台通用的，所以文件路径用`/`就可以了，不要画蛇添足用反斜杠。

CMake会在第一次使用时指定编译器版本，如果后续换了不同的编译器，CMake不会主动去更新，有可能找不到而导致编译失败。这时候，同样按`ctrl+shift+P`，找到`CMake: resetState`重置CMake的扩展状态，重新进行配置，这个时候再指定新的编译器路径就可以了。

## CMake的宏

一些CMake配置文件里会用的宏

|CMake宏|功能|
|:-:|:-:|
|PROJECT_SOURCE_DIR|使用cmake命令后紧跟的目录，一般是工程的根目录|
|PROJECT_BINARY_DIR|执行cmake命令的目录|
|CMAKE_CURRENT_SOURCE_DIR|当前处理的CMakeLists.txt所在的路径|
|CMAKE_CURRENT_BINARY_DIR|target 编译目录|
|EXECUTABLE_OUTPUT_PATH|重新定义目标二进制可执行文件的存放位置|
|LIBRARY_OUTPUT_PATH|重新定义目标链接库文件的存放位置|
|PROJECT_NAME|返回通过PROJECT指令定义的项目名称|
|CMAKE_BINARY_DIR|项目实际构建路径，假设在build目录进行的构建，那么得到的就是这个目录的路径|

## VSCode编译配置文件

这一部分和CMake其实没什么关系，但是同样是配置cpp的，只好放在这边了。因为我不知道怎么用CMake结合VScode进行打断点Debug，所以我实际Debug单文件时，还是使用VSCode自带的编译功能进行Debug的。

VSCode的配置文件在项目目录的`./.vscode/`文件夹下。一般包含4个文件，分别是：

1. `c_cpp_properties.json`:用于配置C/C++语言开发环境的配置文件,它可以用来指定编译器路径、头文件路径、库文件路径等信息。

    ```JSON
    {
        "configurations": [
            {
                "name": "Win32",
                "includePath": [
                    "${workspaceFolder}/**"
                ],
                "defines": [
                    "_DEBUG",
                    "UNICODE",
                    "_UNICODE"
                ],
                "cStandard": "c17",
                "cppStandard": "gnu++14",
                "intelliSenseMode": "windows-gcc-x64",
                // "compilerPath": "C:\\Files\\w64devkit\\bin\\g++.exe",
                "compilerPath": "C:\\Files\\mingw\\bin\\g++.exe",
                "configurationProvider": "ms-vscode.cmake-tools"
            }
        ],
        "version": 4
    }
    ```

2. `settings.json`:这个配置文件不需要自己去设置，当编译链接头文件库时，这个配置文件会自动增加内容。

3. `tasks.json`:用来配置编译的过程。在这里指定好编译器路径和文件路径。

    ```JSON
    {
        "tasks": [
            {
                "type": "shell",
                "label": "g++.exe build active file",
                // "command": "C:\\Files\\w64devkit\\bin\\g++.exe",
                "command": "C:\\Files\\mingw\\bin\\g++.exe",
                "args": [
                    "-g",
                    "${file}",
                    "-o",
                    "${workspaceFolder}\\${fileBasenameNoExtension}.exe"
                ],
                "options": {
                    // "cwd": "C:\\Files\\w64devkit\\bin"
                    "cwd": "C:\\Files\\mingw\\bin"
                },
                "problemMatcher": [
                    "$gcc"
                ],
                "group": "build",
                "detail": "调试器生成的任务。"
            },
        ],
        "version": "2.0.0"
    }
    ```

4. `launch.json`:配置Debug文件。可以设置gdb参数，注意这里最重要的是`preLaunchTask`参数，把它设置为前面的编译任务名字，这样就能实现对文件的编译+Debug。

    ```JSON
    {
        "version": "0.2.0",
        "configurations": [
            {
                "name": "(gdb) 启动",
                "type": "cppdbg",
                "request": "launch",
                "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
                "args": [],
                "stopAtEntry": false,
                "cwd": "${fileDirname}",
                "environment": [],
                "externalConsole": false,
                "MIMode": "gdb",
                // "miDebuggerPath": "C:\\Files\\w64devkit\\bin\\gdb.exe",
                "miDebuggerPath": "C:\\Files\\mingw\\bin\\gdb.exe",
                "setupCommands": [
                    {
                        "description": "为 gdb 启用整齐打印",
                        "text": "-enable-pretty-printing",
                        "ignoreFailures": true
                    },
                    {
                        "description": "将反汇编风格设置为 Intel",
                        "text": "-gdb-set disassembly-flavor intel",
                        "ignoreFailures": true
                    }
                ],
                "preLaunchTask": "g++.exe build active file"
            },
        ]
    }
    ```

---
