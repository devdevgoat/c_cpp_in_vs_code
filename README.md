# c_cpp_in_vs_code
 Setting up C/C++ to compile and run in vscode was a real pain, so cataloging my setup process here. I have done this from scratch yet... steps might be missing. Not sure how I got multiple g++ installations... ugh.

## Setup:

See _.vscode folder for example of below setup. You can copy this folder down and use it, but remove the underscore in the name before adding to your workplace. Also, note 

1. install cygwin
    - https://cygwin.com/install.html
    - add the chere package
    - edit ~/.profile:
        
        ```bash
        export PATH="/c/C/bin:${PATH}"```
2. add  to path:
    ```
        C:\cygwin64\bin
    ```
3. install c/c++ extention
4. edit .vscode/tasks.json:

        ```json
        {
            "version": "2.0.0",
            "tasks": [
            {
                "label": "build dir",
                "type": "shell",
                "command": "mkdir",
                "args": ["-p", "build"],
                "options": {
                "cwd": "${workspaceFolder}"
                }
            },
            {
                "label": "C/C++: g++.exe build active file",
                "type": "shell",
                "command": "g++.exe",
                "args": ["-g", "*.c", "-o", "build/${fileBasenameNoExtension}.exe"],
                "options": {
                "cwd": "${workspaceFolder}"
                },
                "problemMatcher": ["$gcc"],
                "group": {
                "kind": "build",
                "isDefault": true
            },
            "dependsOn":["build dir"]
            }
            ]
        }
        ```
5. edit .vscode/c_cpp_properties.json:

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
                    "windowsSdkVersion": "10.0.17763.0",
                    "compilerPath": "C:\\TDM-GCC-64\\bin\\gcc.exe",
                    "cStandard": "c17",
                    "cppStandard": "c++17",
                    "intelliSenseMode": "gcc-x64"
                }
            ],
            "version": 4
        }```
6. edit .vscode/settings:

        ```json
        {
            "files.associations": {
                "stdio.h": "c"
            },
            "terminal.integrated.shell.windows": "bash.exe",
            // Use this to keep bash from doing a `cd $HOME`
            "terminal.integrated.env.windows": { "CHERE_INVOKING": "1" },
            "terminal.integrated.shellArgs.windows": [ "--login" ]
        }```
7. Setup launch.json:

        ```json
        {
            "version": "0.2.0",
            "configurations": [
                {
                    "name": "gcc.exe build and debug active file",
                    "type": "cppdbg",
                    "request": "launch",
                    "program": "${fileDirname}\\build\\${fileBasenameNoExtension}.exe",
                    "args": [],
                    "stopAtEntry": false,
                    "cwd": "${workspaceFolder}",
                    "environment": [
                        {
                            "name": "PATH",
                            "value": "%PATH%;z:TDM-GCC-64\\bin"
                        }
                    ],
                    "externalConsole": false,
                    "MIMode": "gdb",
                    "miDebuggerPath": "C:\\TDM-GCC-64\\bin\\gdb.exe",
                    "setupCommands": [
                        {
                            "description": "Enable pretty-printing for gdb",
                            "text": "-enable-pretty-printing",
                            "ignoreFailures": true
                        }
                    ],
                    "logging": { "engineLogging": true }, //optional
                    "preLaunchTask": "C/C++: g++.exe build active file"
                }
            ]
        }```
8. reload vscode  