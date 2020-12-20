---
title: Library
---

## [[ROS]] in [[C++]]
### ROS 依赖库
#### 在使用catkin_make的时候，ROS会按照默认的顺序对功能包进行编译。这导致了有的被依赖的包尚未被编译，未生成xxxConfig.cmake文件，因此也无法被其他包的findpackage()命令找到
#### 因此需要利用package.xml文件来提示ROS首先编译依赖包
#### 被依赖库的包
#####
```
catkin_package(
    INCLUDE_DIRS include
    LIBRARIES my_math_lib
)
ib
)
```
#####
## [[cmake]]
##
