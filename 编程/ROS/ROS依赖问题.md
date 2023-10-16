# 使用[[依赖问题#add_dependencies|add_dependencies]]在不同的target之间添加依赖关系

在ROS中,add_dependencies常用于:

- 自动生成代码的依赖设置,如msg、srv自动生成的源代码。
- 自定义库的依赖设置。
- 可执行程序对msg、srv、自定义库的依赖设置。

这样可以确保编译顺序正确,目标程序可以使用依赖的最新代码和库。

总之,add_dependencies在CMake中非常有用,可以避免很多编译链接错误,正确设置target间依赖关系。使用时需要理解目标之间的真正编译构建顺序。

# 常用操作

使用自定义msg时，添加这一行

```cmake
add_dependencies(<target_name> ${PROJECT_NAME}_generate_messages_cpp)
```

使用自定义srv时，添加这一行

```cmake
add_dependencies(<target_name> ${PROJECT_NAME}_gencpp)
```

