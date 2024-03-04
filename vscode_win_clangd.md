# windows vscode clang & clangd 配置

## 准备工作

vscode 安装 clangd 插件
下载clang 17或更高版本
下载ninja

## 准备CMakePresents.json

开始菜单里查找 `x64 Native Tools Command Prompt`相关的命令行工具，在命令行里输入`code`，在这个环境下使用vscode。

工程目录下添加`CMakePresents.json`

```json
{
    "version": 7,
    "configurePresets": [
        {
            "name": "clang-cl",
            "displayName": "Clang-cl 17.0.1 x86_64-pc-windows-msvc",
            "description": "Using compilers: C = E:\\compilers\\LLVM\\bin\\clang-cl.exe, CXX = E:\\compilers\\LLVM\\bin\\clang-cl.exe",
            "binaryDir": "${sourceDir}/out/build/${presetName}",
            "generator": "Ninja",
            "cacheVariables": {
                "CMAKE_INSTALL_PREFIX": "${sourceDir}/out/install/${presetName}",
                "CMAKE_C_COMPILER": "E:/compilers/LLVM/bin/clang-cl.exe",
                "CMAKE_CXX_COMPILER": "E:/compilers/LLVM/bin/clang-cl.exe",
                "CMAKE_BUILD_TYPE": "Debug",
                "CMAKE_EXPORT_COMPILE_COMMANDS": "ON"
            }
        }
    ],
    "testPresets": [
        {
            "name": "clang-cl",
            "description": "",
            "displayName": "",
            "configurePreset": "clang-cl"
        }
    ]
}
```

配置如上所示我们使用clang-cl作为编译器，使用Ninja作为Generator，并且强制生成`compile_commands.json`。
每一个细节都是必要的。

## 准备clangd

工程目录下准备`.clangd`文件，修改其内容为

```clangd
CompileFlags:
    Add : [-std=c++20]
    CompilationDatabase : out/build/clang-cl/
```

启用c++20，并且指定`compile_commands.json`的目录，这样方便clangd去模拟编译，分析语法和源文件依赖。