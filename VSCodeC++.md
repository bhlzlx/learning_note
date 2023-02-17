# Windows上的波浪线问题

* vscode cmake gernerator 选项里使用 Ninja
* cmake kits里使用Clang编译器（换其它的应该也没关系）
* c_cpp_properties.json里配置如下
```json
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
            "windowsSdkVersion": "10.0.19041.0",
            "cStandard": "c17",
            "cppStandard": "c++17",
            "compilerPath": "D:\\compilers\\LLVM\\bin\\clang-cl.exe",
            "intelliSenseMode": "clang-x64",
            "configurationProvider": "ms-vscode.cmake-tools"
        }
    ],
    "version": 4
}
```

一定要带上compilerPath
一般问题就能解决了