## find_package()

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

```
find_package(<name> [major.minor] [EXACT] [QUIET] [REQUIRED] 
             [[COMPONENTS] [components...]] 
             [OPTIONAL_COMPONENTS components...]
             [NO_POLICY_SCOPE])
```

 name - 库的名称,如Boost
 major.minor - 指定版本号
 EXACT - 精确匹配版本
 QUIET - 不输出信息
 REQUIRED - 必须找到,否则报错
 COMPONENTS - 需要查找的特定库,如Boost的日期库

例如:

```
# 定位zlib
find_package(ZLIB) 

# 查找OpenCV 3.4版本 
find_package(OpenCV 3.4 EXACT REQUIRED)

# 查找Boost日期时间组件
find_package(Boost 1.69.0 COMPONENTS date_time)
```

所以find_package通过不同参数可以非常方便地定位第三方库文件,是CMake中重要的命令。