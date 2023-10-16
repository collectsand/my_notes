# 常用参数

***注意：所有参数都可以在vscode的intellisense中找到描述，参数数量不多，所以请认真看大胆写***

- **label**: The task's label used in the user interface.
- **type**: The task's type. For a custom task, this can either be `shell` or `process`. If `shell` is specified, the command is interpreted as a shell command (for example: bash, cmd, or PowerShell). If `process` is specified, the command is interpreted as a process to execute.
- **command**: The actual command to execute.
- **windows**: Any Windows specific properties. Will be used instead of the default properties when the command is executed on the Windows operating system.
- **group**: Defines to which group the task belongs. In the example, it belongs to the `test` group. Tasks that belong to the test group can be executed by running **Run Test Task** from the **Command Palette**.
- **presentation**: Defines how the task output is handled in the user interface. In this example, the Integrated Terminal showing the output is `always` revealed and a `new` terminal is created on every task run.
- **options**: Override the defaults for `cwd` (current working directory), `env` (environment variables), or `shell` (default shell). Options can be set per task but also globally or per platform. Environment variables configured here can only be referenced from within your task script or process and will not be resolved if they are part of your args, command, or other task attributes.
- **runOptions**: Defines when and how a task is

# option中的参数

在 VSCode 的 task.json 中,option 是一个对象,可以设置任务运行时的一些选项。
对于 catkin_make 来说,常用的 option 参数有:
- **cwd - 设置任务的工作目录,必填参数,要设置为 catkin 工作空间的路径。**
- env - 设置额外的环境变量,可以覆盖默认的环境变量。
- shell - 设置使用的 shell,默认是系统默认 shell。

# 创建catkin_make自动构建task

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "catkin_make", 
      "type": "shell", //要执行的命令
      "command": "catkin_make",
      //设置命令参数
      "args": [
        "-DCMAKE_C_COMPILER:FILEPATH=/usr/bin/clang-16",
        "-DCMAKE_CXX_COMPILER:FILEPATH=/usr/bin/clang++-16",
        "--use-ninja",
        "-j8"
      ],
      "options": {
        "cwd": "/path/to/your/catkin_workspace" //设置task工作目录
      },
      "group": "build",// 设置task组，设置成build可以通过ctrl+shift+B快速启动
      "problemMatcher": [],
      "detail": "catkin_make build task"
    }
  ]
}
```

# 踩坑

catkin_make在后台调用ninja来执行实际的编译,所以ninja并不支持catkin_make的"--pkg"参数。**也就是说不要让--use-ninja和--pkg同时出现**

要只编译某个包,需要在catkin_make层面指定,不要把"--pkg"参数传给ninja。