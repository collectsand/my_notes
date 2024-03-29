# CATKIN_DEPENDS

## 基本介绍

CATKIN_DEPENDS在ROS的catkin_package()宏中的作用是指定该软件包依赖的其他catkin软件包。CATKIN_DEPENDS是在catkin编译系统层面指定依赖关系,用于控制不同软件包之间的编译顺序。

举个例子,如果我们的软件包example_pkg依赖于roscpp和std_msgs这两个软件包,那么可以在example_pkg的CMakeLists.txt中这样写:

```cmake
catkin_package(
  ...
  CATKIN_DEPENDS roscpp std_msgs
  ...
)
```

这样一来,在编译example_pkg时,会自动先编译roscpp和std_msgs这两个依赖包,从而确保example_pkg能正常编译通过。

所以简单来说,CATKIN_DEPENDS就是声明该软件包依赖的其他catkin包,catkin会根据这个依赖关系进行编译。指定依赖可以避免编译错误,也有利于提高编译效率。

**有些时候一些教程catkin_package()中啥都不写(~~说的就是官方tutorials~~)，虽然也能构建成功，但到了单独构建时就会出问题！**

## 底层原理

catkin_package()中的CATKIN_DEPENDS用于指定一个ROS软件包需要依赖的其他catkin软件包。其底层原理是:

1. catkin_package()会生成一个\<package>.cmake文件,里面包含有该软件包的依赖信息。

2. 当编译一个catkin工作空间时,CMake会读取每个软件包的\<package>.cmake文件,从而构建出整个工作空间的依赖关系图。 

3. CMake会根据依赖关系图正确地编译和链接所有的软件包。依赖的软件包会先于被依赖的软件包进行编译。

4. 在链接目标时,CMake也会将依赖包的库文件正确地链接上。

所以CATKIN_DEPENDS的作用就是声明该软件包依赖哪些其他catkin包,这样CMake可以根据依赖关系图进行正确的编译和链接,确保不同软件包之间的依赖关系得到满足。

## 注意事项

**CATKIN_DEPENDS** 和 **package.xml** 中的\<run_depend>部分是对应的，比如message_generation

```xml
<run_depend>message_generation</run_depend>
```

仅在构建时有用的包写在 **CATKIN_DEPENDS** 会报错

```xml
<build_depend>message_generation</build_depend>
```

cmake报错提示要指定为 run dependency

```
[cmake] CMake Error at /opt/ros/noetic/share/catkin/cmake/catkin_package.cmake:224 (message):

[cmake]   catkin_package() DEPENDS on the catkin package 'message_generation' which

[cmake]   must therefore be listed as a run dependency in the package.xml
```

***另外 roscpp、rospy 和 std_msgs 这些都是默认被包含在run_depend中的ROS核心包,即使没有明确声明也会被自动依赖。***

catkin会默认将metapackage(如ros_base)依赖的所有包加入run_depend。而ros_base中包含了roscpp、rospy、std_msgs等核心ROS包。

所以这些包可以直接在CATKIN_DEPENDS中使用,无需再在package.xml中声明run_depend。

综上,出现在CATKIN_DEPENDS中的包有两种情况:

1. 默认包含的ROS核心包,如roscpp、rospy等。这些可以直接使用。
2. 非核心包的依赖,这些必须在package.xml中用build_depend或run_depend声明,否则会编译错误。

所以 message_generation就属于后者,需要显式声明依赖。而roscpp、rospy等属于前者,无需声明。