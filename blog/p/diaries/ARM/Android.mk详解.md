#Android.mk详解

---
本文整理自[飞奔的蜗牛](https://huangxiankui.github.io/blog/index.html?diaries/index.md)，转载请加以说明

---
Android.mk是Android工程管理文件，其作用基本等用于Linux环境中的Makefile,在语法上，Android.mk和普通Makefile略有不同，主要区别是Android.mk包含一些Android编译系统的公共的宏，Android.mk中选项参考以下文件路径: build/core/config.mk，各个选项的默认值在以下文件中定义：
 
build/core/base_rules.mk
 
Android.mk文件只处理从根目录开始找到的第一个Android.mk文件，如果需要递归。需要在当前目录的Android.mk文件中做如下处理：
 
include $(call all-makefiles-under,$(LOCAL_PATH))
 
在一个Android.mk中也可以生成多个目标：可执行程序，动态库，静态库或者Android用程序包，Android.mk文件可以处理多个内容：
 
include $(CLEAR_VARS) //清理变量 

---
 
##处理第一个内容
 
include $(CLEAR_VARS)

---
##处理第二个内容
 
现在我们来讨论对于各种目标的构建方式
 
###1、可执行程序

```c
LOCAL_PATH :=$(my_dir) //本地路径，当前目录
 
Text EXe
 
include $(CLEAR_VARS)
 
LOCAL_SRC_FILES := eng
 
LCAOL_SRC_FILES :=
 
main.c
 
LOCAL_MODULE := text_exe
 
LOCAL_C_INCLUDES :=
 
LOCAL_STATIC_LIBRARIES :=
 
LOCAL_SHARED_LIBRARIES := libc
 
include $(BUILD_EXECUTABLE)
```
注：编译一个可执行程序，需要在LOCAL_SRC_FILES 中假如远见路径(相对于当前Android.mk目录的路径)，
 
在LOCAL_C_INCLUDES中假如所需要包含的头文件路径；
 
在LOCAL_STATIC_LIBRARIES 中假如所需要链接的静态库（*.a)的名称；
 
在LOCAL_SHARED_LIBRARIES中假如所需要链接的动态库(*.so)的名称；
 
LOCAL_MODULE标示模块最终的名称，最后使用include$（build_executable），表示以一个可执行程序的方式进行编译，在本例中，LOCAL_MODULE被定义为test_exe,因此最终生成可执行程序的名称是test_exe，一个可执行程序编译后生成独立的目标目录在out/target/product/中，路径如下所示：
 
/obj/EXECUTABLES/
 
###2、静态库
 
静态库，也称之为归档文件，在Linux中扩展名通常为.a,在Android.mk中编译一个静态库（归档文件）的模板如下所示：
```c
LOCAL_PATH :=$(my-dir)
 
include $(CLEAR_VARS)
 
LOCAL_MODULE_TAGS := eng 
 
LOCAL_SRC_FILES :=
 
hello.c
 
LOCAL_C_INCLUDES :=
 
LOCAL_STATIC_LIBARIES :=
 
LOCAL_SHARED_LIBRARAIES := libc
 
include $(BUILD_STATIC_LIBRARY)
```
在编译静态库，基本的内容和编译可执行程序相似，区别在于include$(BUILD_STATIC_LIBRARY) 标示编译静态库。在本例中，LOCAL_MODULE被定义为libtest_static,因此最终生成的静态库的名称是libtest_static.a。
 
###3、动态库
 
动态库 也称之为共享库，是linux标准的elf格式文件的一种，在Linux中扩展名通常为.so在Android中编译一个动态库（共享库）的模板如下：
```c
LOCAL_PATH := $(my-dir)
 
include $ (CLEAR_VARS)
 
LOCAL_MODULE_TAGS :=eng
 
LOCAL_SRC_FILES := 
 
hello.c
 
LOCAL_MODULE := libtest_stared
 
TARGET_PRELINK_MODULE := false
 
LOCAL_C_INCLUDES :=
 
LOCAL_STATIC_LIBRARIES :=
 
LOCAL_SHARED_LIBRARIES := libc
 
include $ (BUILD_SHARED_LIBRARY)
```
编译一个动态库，基本的内容和编译可执行程序、静态库相似，区别在于使用include$(BUILD_SHARED_LIBRARY) 表示编译动态库。在本例中，LOCAL_MODULE被定义为libtest_shared,因此最终生成的动摇库的名称是：libtest_shared.so 一个动态库编译后生成的独立的目标目录在out/target/product/中，路径如下所示：
 
/obj/STATIC_LIBRARIES/
 
Android 应用程序包是一种特殊的文件，通常以apk为扩展名。在Android.mk中编译一个应用程序包的模板如下所示：
```c 
LOCAL_PATH := $(CALL my-dir)
 
include $(CLEAR_VARS)
 
LOCAL_MODULE_TAGS := eng
 
LOCAL_PACKAGE_NAME := TestApplication
 
include $(BUILD_PACKAGE)
```
这里使用BUILD_PACKAGE宏表示编译apk，而LOCAL_SRC_FILES 使用自动查找的方法，将找打所有的java文件进行编译， 在源代码环境中编译和在SDK中编译应用程序包略有不同，涉及的目录主要有一下两个。 out/target/common/obj/APPS: 通用java字节码目录
 
out/target/product/obj/APPS:Android 应用包目录
 
每个包在两个目录中均具有名为{LOCAL_PACKAGE_NAME}_intermediates/的独立目录 例如，对于SkeletonApp包编译，公共目录的生成结构如下所示：
 
关于在编译应用程序的时候，有一个额外的宏可以控制编译的行为，如下所示：
 
WITH_DEXPREOPT := true
 
如果WITH_DEXPREOPT 被定义为true,一个应用程序将由两个部分组成：一个是不包含java字节码classes.dex的apk文件，一个是名称为classes.odex的字节码文件。
 
两个文件同时预制在系统中依然可以构成能运行的应用程序。但是，这种生成的结构就不能在进行动态的安装了。
 
Android4.x以上默认为true
 
LOCAL_MODULE := hello-jni
 
因为模块名称也被用于给构建过程所生成的文件命名，所以构建系统给该文件添加了适当的前缀和后缀，本例中，hello-jni模块汇申城一个共享库文件且构建系统会将他命名为hello-jni.so文件，每一个模块的写法都基本类似比如：
```c 
include $(CLEAR_VARS)
 
LOCAL_MODULE := MODULE1
 
LOCAL_SRC_FILES : = MODUE1.C MODULE2.C(这里可以添加多个文件.c)
 
INCLUDE $(BUILD_SHARED_LIBRARY)
```
关于添加构建的静态库
 
Android NDK 构建系统也支持静态库。实际的Android应用程序并不直接使用静态库，并且应用程序包中也不包含静态库。静态库可以用来构建共享库，例如：在将第三方代码添加到现有原生项目中时，不用直接将第三方源代码包括在原生项目中，而是将第三方编译成静态库然后并入共享库。
 
代码：
```c
 
LOCAL_PATH := $(call my-dir)
 
inlcude $(CLEAR_VARS)
 
LOCAL_MODULE :=avilib
 
LOCAL_SRC_FILES := avilib.c platform_posix.c
 
include $(BUILD_STATIC_LIBRARY)

include $(CLEAR_VARS)
 
LOCAL_MODULE := MODULE
 
LOCAL_SRC_FILES :=MODULE.C
 
LOCAL_STATIC_LIBRARIES := avilib
 
include $(BUILD_SHARED_LIBRARY)
```
在将第三方代码模块生成静态库之后，共享库就可以通过讲它的模块名添加到LOCAL_STATIC_LIBRARIES变量中来使用该模块。
 
用共享库共享通用模块
 
静态库可以保证源代码模块化：但是，当静态库与共享库相连是，它就变成了共享库的一部分，在多个共享库的情况下，多个共享库与同一个静态库链接时，需要将通用模块的多个副本与不同共享库重复项链，这样就增加了应用程序的大小，在这种情况下，不用构建静态库而是将通用模块作为共享库建立起来，而动态链接依赖模块以便消除重复的副本 Android.mk 问价那种共享库之间的代码共享。
```c
 
LOCAL_PATH := $(call my-dir)
 
include $(CLEAR_VARS)
 
LOCAL_MODULE :=avilib
 
LOCAL_SRC_FILES := avilib.c platform_posix.c
 
include $(BUILD_SHARED_LIBRARY)
 
include $(CLEAR_ VARS)
 
LOCAL_MODULE := MODULE2
 
LOCAL_SRC_FILES := MODULE2.C
 
LOCAL_SHARED_LIBRARIES := avilib
 
include $(BUILD_SHARED_LIBRARY) 
```