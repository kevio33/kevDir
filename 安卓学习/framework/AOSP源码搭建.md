> 参考——
>
> [环境搭建](https://www.cnblogs.com/EthanS/p/18211302)
>
> [配置过程](https://www.bilibili.com/video/BV14g4y1x7Eo/?spm_id_from=333.1387.collection.video_card.click&vd_source=393163f970d7c1ef4544ef8af35b3690)

## 1.开发环境搭建

- 虚拟机——[VMWare](https://www.cnblogs.com/EthanS/p/18211302)

- 镜像——[Ubuntu 20.04.6 LTS (Focal Fossa)](https://releases.ubuntu.com/focal/)

> 环境搭配参考上面[配置过程](https://www.bilibili.com/video/BV14g4y1x7Eo/?spm_id_from=333.1387.collection.video_card.click&vd_source=393163f970d7c1ef4544ef8af35b3690)和[图文](https://github.com/yuandaimaahao/AndroidFrameworkTutorial/blob/main/2.AOSP%E4%B8%8A%E6%89%8B%E6%8C%87%E5%8D%97/001.AOSP%20%E6%9E%81%E9%80%9F%E4%B8%8A%E6%89%8B.md)

- 下载linux需要的软件包：

```shell
sudo apt-get install git-core gnupg flex bison build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 libncurses5 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig python
```

## 2.下载，编译源码

### 下载repo工具

```shell
mkdir ~/bin
curl https://mirrors.tuna.tsinghua.edu.cn/git/git-repo -o ~/bin/repo
chmod +x ~/bin/repo
```

> **注意：**
>
> repo 的运行过程中会尝试访问官方的 git 源更新自己，如果想使用 tuna 的镜像源进行更新，可以**将如下内容复制到 ~/.bashrc 或者 ~/.zshrc 里**。 
>
> ```shell
> export REPO_URL='https://mirrors.tuna.tsinghua.edu.cn/git/git-repo'
> PATH=~/bin:$PATH
> ```
>
> 然后source执行，使更改的全局变量生效
>
> ```shell
> source ~/.bashrc
> #如果使用的是 zsh
> #source ~/.zshrc
> ```

### 初始化仓库并同步远程代码

```shell
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
mkdir aosp 
cd asop
#初始化仓库,-b 指示分支，这里使用 android10
repo init -u https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/manifest -b android-10.0.0_r41
#同步远程代码
repo sync
```

> 这一步花费的时间相当长，2个小时左右

结束之后的目录：
![1748915192081](AOSP源码搭建.assets/1748915192081.png)

### 编译源码

```shell
source build/envsetup.sh 

lunch  
26. aosp_x86_64-eng #选择x86

#或者直接选择指定的编译版本
lunch aosp_x86_64-eng
```

- 命令一：加载 AOSP 编译环境：该脚本定义了编译 Android 所需的函数、环境变量和别名（如 mm、mma 等快捷命令）。
- 命令二：选择目标编译配置：指定编译的 Android 版本、CPU 架构和构建类型。eng表示构建类型为 **Engineer (eng)**，包含调试工具和 root 权限。 



然后使用`make`来编译

```shell
m #自动根据cpu情况选择线程数

make -j16 #指定16线程
```

> 第一次差不多2个半小时

然后使用emulator命令起模拟器和Android系统：

```shell
emulator -verbose -cores 4 -show-kernel
#运行模拟器可以直接emulator命令
```

> **首次启动一定要带这几个参数，否则起不起来**

![1748926053374](AOSP源码搭建.assets/1748926053374.png)



### 初次修改源码

**首次简单的修改源码，去掉谷歌搜索框**

> 通过vscode远程连接，连接过程参考vscode笔记

修改 `packages/apps/Launcher3/res/layout/search_container_workspace.xml`，将以下内容注释掉： 

```xml
<fragment
          android:name="com.android.launcher3.qsb.QsbContainerView$QsbFragment"
          android:layout_width="match_parent"
          android:tag="qsb_view"
          android:layout_height="match_parent"/>
```

修改文件： `packages/apps/Launcher3/src/com/android/launcher3/Workspace.java` 将以下内容注释掉 

```shell
CellLayout.LayoutParams lp = new CellLayout.LayoutParams(0, 0, firstPage.getCountX(), 1);
lp.canReorder = false;
if (!firstPage.addViewToCellLayout(qsb, 0, R.id.search_container_workspace, lp, true)) {
     Log.e(TAG, "Failed to add to item at (0, 0) to CellLayout");
 }
```

再重新编译运行



## 3.常用开发工具

- Tabby：ssh客户端工具
- Vim
- samba服务器搭建： 一款数据共享的软件，可用于 Ubuntu 与 Windows 之间共享源代码 
- AIDEGen+AndroidStudio

### AIDEGen+AS

首先进入源码目录，编译sdk

```shell
source build/envsetup.sh
lunch sdk-eng
# or
#lunch sdk-userdebug
# or
#lunch sdk-user
make sdk
```

> **这个编译sdk一直不成功，不知道是内存不足原因还是什么，直接放弃**

## 4.Product

在 App 的开发中，要去打渠道包，根据不同应用市场的要求打包出不同的 apk 包。

同样的道理，Android 的系统源码，经过简单的配置，可以打包出不同的系统镜像，用于不同的产品。 配置文件成为product，例如前面编译的时候的：`aosp_x86_64-eng`就属于一个product。

![img](AOSP源码搭建.assets/68747470733a2f2f67697465652e636f6d2f7374696e6765727a6f752f7069632d6265642f7261772f6d61737465722f696d672f32303233303232333230313234362e706e67.png) 



### aosp预制product

#### 通过`lunch`命令，可以查看aosp预制的product列表

```shell
lunch

You're building on Linux

Lunch menu... pick a combo:
     1. aosp_arm-eng
     2. aosp_arm64-eng
     3. aosp_blueline-userdebug
     4. aosp_bonito-userdebug
     5. aosp_car_arm-userdebug
     6. aosp_car_arm64-userdebug
     7. aosp_car_x86-userdebug
     8. aosp_car_x86_64-userdebug
     9. aosp_cf_arm64_phone-userdebug
     10. aosp_cf_x86_64_phone-userdebug
     11. aosp_cf_x86_auto-userdebug
     12. aosp_cf_x86_phone-userdebug
     13. aosp_cf_x86_tv-userdebug
     14. aosp_coral-userdebug
     15. aosp_coral_car-userdebug
     16. aosp_crosshatch-userdebug
     17. aosp_crosshatch_car-userdebug
     18. aosp_flame-userdebug
     19. aosp_marlin-userdebug
     20. aosp_sailfish-userdebug
     21. aosp_sargo-userdebug
     22. aosp_taimen-userdebug
     23. aosp_walleye-userdebug
     24. aosp_walleye_test-userdebug
     25. aosp_x86-eng
     26. aosp_x86_64-eng
     27. beagle_x15-userdebug
     28. car_x86_64-userdebug
     29. fuchsia_arm64-eng
     30. fuchsia_x86_64-eng
     31. hikey-userdebug
     32. hikey64_only-userdebug
     33. hikey960-userdebug
     34. hikey960_tv-userdebug
     35. hikey_tv-userdebug
     36. m_e_arm-userdebug
     37. mini_emulator_arm64-userdebug
     38. mini_emulator_x86-userdebug
     39. mini_emulator_x86_64-userdebug
     40. poplar-eng
     41. poplar-user
     42. poplar-userdebug
     43. qemu_trusty_arm64-userdebug
     44. uml-userdebug

Which would you like? [aosp_arm-eng]
```



**product文件主要存在两个目录下面：**

- build/target：存放模拟器相关的product文件
- device：存放芯片以及方案厂商提供的product配置文件

查看`build/target`目录下的结构：

- board：主要是**硬件相关**配置
- product：主要是产品相关配置

![1749281451939](AOSP源码搭建.assets/1749281451939.png)

> 也可以通过tree命令查看目录结构
>
> ```shell
> tree ./build/target -L 2
> ```



**选择aosp_x86_64-eng，主要关注下面文件：**

- `/board/generic_x86_64/BoardConfig.mk` ： 用于硬件相关配置，包括硬件芯片架构配置、分区大小等等
- `/product/AndroidProducts.mk` `/product/aosp_x86_64.mk`：用于配置 Product



**`/board/generic_x86_64/BoardConfig.mk`**内容：

```makefile
# x86_64 emulator specific definitions
TARGET_CPU_ABI := x86_64
TARGET_ARCH := x86_64
TARGET_ARCH_VARIANT := x86_64

TARGET_2ND_CPU_ABI := x86
TARGET_2ND_ARCH := x86
TARGET_2ND_ARCH_VARIANT := x86_64

TARGET_PRELINK_MODULE := false
include build/make/target/board/BoardConfigGsiCommon.mk #通用系统映像的配置
include build/make/target/board/BoardConfigEmuCommon.mk #模拟器的配置

BOARD_USERDATAIMAGE_PARTITION_SIZE := 576716800

BOARD_SEPOLICY_DIRS += device/generic/goldfish/sepolicy/x86

# Wifi.
BOARD_WLAN_DEVICE           := emulator
BOARD_HOSTAPD_DRIVER        := NL80211
BOARD_WPA_SUPPLICANT_DRIVER := NL80211
BOARD_HOSTAPD_PRIVATE_LIB   := lib_driver_cmd_simulated
BOARD_WPA_SUPPLICANT_PRIVATE_LIB := lib_driver_cmd_simulated
WPA_SUPPLICANT_VERSION      := VER_0_8_X
WIFI_DRIVER_FW_PATH_PARAM   := "/dev/null"
WIFI_DRIVER_FW_PATH_STA     := "/dev/null"
WIFI_DRIVER_FW_PATH_AP      := "/dev/null"
```



**`/product/AndroidProducts.mk`**

主要是定义了lunch所选择编译目标的判断逻辑

```mak
ifneq ($(TARGET_BUILD_APPS),)
PRODUCT_MAKEFILES := \
    $(LOCAL_DIR)/aosp_arm64.mk \
    $(LOCAL_DIR)/aosp_arm.mk \
    $(LOCAL_DIR)/aosp_x86_64.mk \
    $(LOCAL_DIR)/aosp_x86.mk \
    $(LOCAL_DIR)/full.mk \
    $(LOCAL_DIR)/full_x86.mk \

else
PRODUCT_MAKEFILES := \
    $(LOCAL_DIR)/aosp_arm64_ab.mk \
    $(LOCAL_DIR)/aosp_arm64.mk \
    $(LOCAL_DIR)/aosp_arm_ab.mk \
    $(LOCAL_DIR)/aosp_arm.mk \
    $(LOCAL_DIR)/aosp_x86_64_ab.mk \
    $(LOCAL_DIR)/aosp_x86_64.mk \
    $(LOCAL_DIR)/aosp_x86_ab.mk \
    $(LOCAL_DIR)/aosp_x86_arm.mk \
    $(LOCAL_DIR)/aosp_x86.mk \
    $(LOCAL_DIR)/full.mk \
    $(LOCAL_DIR)/full_x86.mk \
    $(LOCAL_DIR)/generic.mk \
    $(LOCAL_DIR)/generic_x86.mk \
    $(LOCAL_DIR)/gsi_arm64.mk \
    $(LOCAL_DIR)/mainline_arm64.mk \
    $(LOCAL_DIR)/mainline_system_arm64.mk \
    $(LOCAL_DIR)/sdk_arm64.mk \
    $(LOCAL_DIR)/sdk.mk \
    $(LOCAL_DIR)/sdk_phone_arm64.mk \
    $(LOCAL_DIR)/sdk_phone_armv7.mk \
    $(LOCAL_DIR)/sdk_phone_x86_64.mk \
    $(LOCAL_DIR)/sdk_phone_x86.mk \
    $(LOCAL_DIR)/sdk_x86_64.mk \
    $(LOCAL_DIR)/sdk_x86.mk \

endif

COMMON_LUNCH_CHOICES := \
    aosp_arm64-eng \ 
    aosp_arm-eng \
    aosp_x86_64-eng \
    aosp_x86-eng \
```

COMMON_LUNCH_CHOICES 用于添加 lunch 时的选项，选项的名字由两部分过程 `产品名 + 构建模式`：

- 产品名就是 PRODUCT_MAKEFILES 中引入的产品配置文件名去掉 `.mk` 后缀，例如 aosp_x86_64

- **构建模式**有三种：用户模式 user、用户调试模式 userdebug 和工程模式 eng

  > | 特性                               | `eng`                      | `userdebug`                  | `user`           |
  > | ---------------------------------- | -------------------------- | ---------------------------- | ---------------- |
  > | 编译内容                           | 包含全部开发工具、测试接口 | 接近 user，额外加 debug 特性 | 精简、关闭 debug |
  > | ADB 默认开启                       | ✅（无需授权）              | ✅（需授权）                  | ✅（需授权）      |
  > | Root 权限支持                      | ✅                          | ✅                            | ❌                |
  > | 调试命令（如 `adb root`）          | ✅ 无限制                   | ✅ 有限制                     | ❌ 禁止使用       |
  > | 默认编译选项（如 `ro.debuggable`） | `1`                        | `1`                          | `0`              |
  > | 是否允许修改系统属性               | ✅                          | ✅                            | ❌                |
  > | 是否适合日常用户使用               | ❌ 仅开发者使用             | ❌ 部分适合测试               | ✅ 推荐发布版本   |
  > | 默认打开的 SELinux 模式            | `permissive`               | `enforcing`                  | `enforcing`      |

> **上面是.mk文件的代码，涉及GNU make的语法，这里不具体说明了**

### 自制Product

假设公司名叫Kevin，准备开发一款手机名叫Rice16

首先再device目录下添加如下目录和文件：

```shell
Kevin/
└── Rice16
    ├── AndroidProducts.mk
    ├── BoardConfig.mk
    └── Rice16.mk
```

- `BoardConfig.mk`配置直接拷贝build/target/board/generic_x86_64中`BoardConfig.mk`内容即可

- `Rice16.mk`拷贝` build/target/product/aosp_x86_64.mk `

  > 需要注释掉其中的if判断、修改最后四行配置的名称
  >
  > ```makefile
  > 
  > PRODUCT_USE_DYNAMIC_PARTITIONS := true
  > 
  > # GSI for system/product
  > $(call inherit-product, $(SRC_TARGET_DIR)/product/core_64_bit.mk)
  > $(call inherit-product, $(SRC_TARGET_DIR)/product/gsi_common.mk)
  > 
  > # Emulator for vendor
  > $(call inherit-product-if-exists, device/generic/goldfish/x86_64-vendor.mk)
  > $(call inherit-product, $(SRC_TARGET_DIR)/product/emulator_vendor.mk)
  > $(call inherit-product, $(SRC_TARGET_DIR)/board/generic_x86_64/device.mk)
  > 
  > # Enable mainline checking for excat this product name
  > # ifeq (aosp_x86_64,$(TARGET_PRODUCT))
  > PRODUCT_ENFORCE_ARTIFACT_PATH_REQUIREMENTS := relaxed
  > # endif
  > 
  > PRODUCT_ARTIFACT_PATH_REQUIREMENT_WHITELIST += \
  >     root/init.zygote32_64.rc \
  >     root/init.zygote64_32.rc \
  > 
  > # Copy different zygote settings for vendor.img to select by setting property
  > # ro.zygote=zygote64_32 or ro.zygote=zygote32_64:
  > #   1. 64-bit primary, 32-bit secondary OR
  > #   2. 32-bit primary, 64-bit secondary
  > # init.zygote64_32.rc is in the core_64_bit.mk below
  > PRODUCT_COPY_FILES += \
  >     system/core/rootdir/init.zygote32_64.rc:root/init.zygote32_64.rc
  > 
  > PRODUCT_NAME := Kevin
  > PRODUCT_DEVICE := Rice16
  > PRODUCT_BRAND := Rice16
  > PRODUCT_MODEL := AOSP on x86_64 Rice16
  > ```

- ` AndroidProducts.mk `内容：

  ```makefile
  PRODUCT_MAKEFILES :=\
  	$(LOCAL_DIR)/Rice16.mk
  
  COMMON_LUNCH_CHOICES :=\
  	Rice16-eng \
  	Rice16-userdebug \
  	Rice16-user
  ```

然后进行验证：

![1749285734091](AOSP源码搭建.assets/1749285734091.png)

## 5.添加可执行程序

### 前言：ARM+Android行业开发流程

从aosp源码开始，基于ARM的底层代码涉及封装流程可以简单归纳如下：

1. Google 开发迭代 AOSP + Kernel
2. 芯片厂商， （如 Qualcomm、MTK、Rockchip） 针对自己的芯片特点，移植 AOSP 和 Kernel，使其可以在自己的芯片上跑起来。
3. 方案厂商（很多芯片厂商也扮演了方案厂商的角色）ODM，设计电路板，给芯片添加外设，在芯片厂商源码基础上开发外设相关的驱动和 HAL，改进性能和稳定性。
4. 产品厂商，主要是系统软件开发，UI 定制以及硬件上的定制(添加自己的外设)，改进性能和稳定性.

> 对于上面提到的ODM和OEM，GPT给出的解释是：
>
> **ODM（设计与制造）**
>
> - 承接品牌商（或 ODM 自己也会有白牌业务）的需求，负责整块板卡、SoC 方案的原始设计与初步验证。
> - 通常与芯片厂商（Qualcomm、MediaTek、Unisoc 等）和模具厂紧密合作。
> - 完成硬件原型后，把设计资料（Gerber、BOM、驱动源码、设备树）交给 OEM。
>
> **OEM（品牌与整机）**
>
> - 以自己的品牌面向市场，采购 ODM（或自己内部）设计出的硬件，做外观、包装、品牌定制。
> - 负责系统整合：将 AOSP、第三方库和应用、品牌定制 UI/服务打包成最终固件。
> - 做认证、量产测试、物流和售后。
>
>  **ODM 聚焦硬件设计，OEM 聚焦整机品牌和系统定制。** 



### Android分区

常用的四个分区：

- System 分区：

  Google 开发的通用 Android 系统组件编译后会被存放到 System 分区，原则上不同厂商、不同型号的设备都通用。 

- Vender 分区：

   **芯片厂商**和**方案厂商**针对硬件相关的平台通用的可执行程序、库、系统服务和 app 等一般放到 Vender 分区。（部分内核驱动程序是放在 boot 分区的 kernel 部分） 

- Odm 分区：

    存放不同产品板型适配的`硬件`差异化组件 （例如谷歌开发的产品A用的京东方屏，产品B用的三星屏）。公用部分放到vender

- Product 分区：

   存放不同产品线、品牌、地区的定制`软件`内容（如 Launcher、UI 风格、预装应用、品牌服务等；例如：产品A带广告，产品B不带广告）；公用的部分放在System分区





