# find_package()

## 基本用法

find_package()是CMake中用于定位第三方库的命令。它的主要作用有:

1. 定位第三方库的文件路径

find_package通过查找路径或者环境变量,来确定第三方库的头文件和库文件路径,并将其保存到对应的CMAKE_XXX_INCLUDE_DIR和CMAKE_XXX_LIBRARIES变量中。

2. 导入配置文件

有些第三方库会提供FindXXX.cmake模块,find_package会导入这个模块,通过模块设置相关变量。

3. 版本检查

可以指定版本要求,find_package会检查第三方库版本是否满足。

4. 导入库目标

现代CMake通过export导出目标,find_package通过查找XXXConfig.cmake文件来导入目标。

基本语法:

```cmake
find_package(<name> [major.minor] [EXACT] [QUIET] [REQUIRED] 
             [[COMPONENTS] [components...]] 
             [OPTIONAL_COMPONENTS components...]
             [NO_POLICY_SCOPE])
```

 name - 库的名称,如Boost
 major.minor - 指定版本号
 EXACT - 精确匹配版本
 QUIET - 不输出信息
 **REQUIRED - 必须找到,否则报错**
 **COMPONENTS - 从库中找子库（模块），需要查找的特定库,如Boost的日期库**

例如:

```cmake
# 定位zlib
find_package(ZLIB) 

# 查找OpenCV 3.4版本 
find_package(OpenCV 3.4 EXACT REQUIRED)

# 查找Boost日期时间组件
find_package(Boost 1.69.0 COMPONENTS date_time)
```

所以find_package通过不同参数可以非常方便地定位第三方库文件,是CMake中重要的命令。



## 原理相关

### find_package()到底干了啥？

What Does find_package() Do?
If a package is found by CMake through find_package, it results in the creation of several CMake environment variables that give information about the found package. These environment variables can be utilized later in the CMake script. The environment variables describe where the packages exported header files are, where source files are, what libraries the package depends on, and the paths of those libraries. The names always follow the convention of ==PACKAGE NAME_PROPERTY==:

* ==NAME_FOUND== - Set to true if the library is found, otherwise false
* ==NAME_INCLUDE_DIRS== or ==NAME_INCLUDES== - The include paths exported by the package 
* ==NAME_LIBRARIES== or ==NAME_LIBS== - The libraries exported by the package
* ==NAME_DEFINITIONS== - ?

 参见 http://wiki.ros.org/catkin/CMakeLists.txt 



## Q&A

***一、find_package()能直接把头文件添加进去吗？为什么find完了以后还得使用target_link_library()来连接？***

find_package()不能直接把头文件添加到工程中,它主要用于定位第三方库的路径。连接库还需要使用target_link_libraries()。

这是因为find_package()仅仅是找到了第三方库的路径,但还没有把这个库actually添加到工程中去。它通常会设置两个变量:

- XXX_INCLUDE_DIRS - 库的头文件目录
- XXX_LIBRARIES - 库的库文件路径

这就包含了第三方库的位置信息,但还没有引入工程。

为什么需要区分这两个步骤呢?

1. 分离定位和链接可以更灵活地控制include和link的顺序
2. target_link_libraries与导入的target捆绑,可以避免全局污染
3. 将第三方库封装为target,可以优化编译流程

所以find_package()+target_link_libraries()是现代CMake推荐的方式,通过导入目标进行链接。

一个现代CMake的用法是:

```cmake
find_package(OpenCV REQUIRED) 
target_link_libraries(myApp PRIVATE OpenCV::opencv)
```

通过IMPORTED目标opencv进行连接,这样就可以自动处理好依赖关系了。

所以find_package只是定位,真正使用第三方库还需要target_link_libraries。这是现代CMake连接第三方库的推荐方式。

***二、那为啥不需要使用target_include_directory()来引入头文件？***

>在CMake中,find_package后通常不需要再显式使用target_include_directories来添加头文件目录,这是因为现代CMake通过导入目标(IMPORTED targets)已经自动处理了依赖头文件。

原因有以下几点:

1. target_include_directories是用来将include目录添加到特定目标(target)的,而不是整个项目。但导入的目标已经包含了头文件信息,可以直接使用,不需要再添加到其他目标中。

2. 导入的目标里已经包括了对应的include目录,CMake能自动处理目标之间的依赖,所以可以不用再手动添加include。

3. 现代CMake提倡面向目标编程,而不是全局include目录,所以尽量避免全局include污染。

4. 一些编译系统如Visual Studio,可以通过导入目标自动获取include目录,不需要手动指定。

以OpenCV库为例:

```cmake
find_package(OpenCV REQUIRED) 

# 不需要再添加include目录
target_link_libraries(myApp OpenCV::opencv)
```

通过导入opencv目标,CMake就能自动处理好依赖的include目录,不需要也不推荐再另外添加include。

但是,如果目标导入有问题,或者跨平台移植,可能还是需要手动添加include目录的。但对于现代CMake,能够不使用target_include_directories则最好。

所以导入目标后不需要再手动include头文件目录,是现代CMake面向目标编程的一大特点。