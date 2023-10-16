
***概括：首先使用find_package()来定位第三方库，然后使用target_link_libraries()导入第三方库***

好的,关于CMake中导入目标的用法,我来给一个具体的 C++ 导入第三方库的例子:

1. 使用 find_package 寻找第三方库

```cmake
# 查找 OpenCV 库
find_package(OpenCV REQUIRED)
```

2. 通过导入配置文件获取库的目标信息

find_package 会通过 OpenCVConfig.cmake 获取 OpenCV 库导出的目标,比如 opencv_core,opencv_imgproc 等。

3. 使用目标链接到项目

```cmake
# 设置可执行文件 myapp
add_executable(myapp myapp.cpp) 

# 链接 OpenCV 库目标
target_link_libraries(myapp PRIVATE OpenCV::opencv_core OpenCV::opencv_imgproc)
```

这里通过导入的目标 OpenCV::opencv_core 来进行链接,相当于自动处理好了依赖关系和相关的 include 目录、库文件等信息。

4. 生成项目文件

```cmake
# 生成 Visual Studio 项目
cmake -G "Visual Studio 16 2019" ..
```

到此就完成了通过导入目标的方式链接 OpenCV 库。整个过程使用了 find_package,target_link_libraries 和导入的目标,避免了include目录和库文件的硬编码。

这就是现代CMake面向目标的编程方式,通过导入配置文件获取目标信息,然后使用目标进行链接,可以简化依赖管理,是C++项目中使用第三方库的推荐方式。