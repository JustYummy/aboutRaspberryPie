## ⚠️[友情提醒] 本教程从网上收集总结得来，由于安装耗时过长并没有验证过，只是作为学习材料
### 如何在树莓派3B上装Windows10 arm

#### 需要材料：

- 树莓派3B
- 16G以上的SD卡
- Windows 10 arm 镜像
- microSD卡转换器

##### 步骤一:

- 格式化SD卡
- 使用[DiskGenius](http://www.diskgenius.cn/download.php)创建两个分区，第一个100MB格式为FAT32，将剩下的容量格式化为NTFS格式的盘符（分区表用默认的MBR）

##### 步骤二：

- 前往[UUP dump](http://mdluup.ct8.pl/index.php)下载镜像下载工具，进入首页后选择`known build`，接着选择一个arm64版本，之后选择Language语言和Edition版本以后进入下载界面，下载方式选择`Dowdload using aria2 and then convert`，之后回下载一个压缩包
- 将压缩包解压，里面有个`aria2_download.cmd`的命令行工具，运行等待结束获得一个封装好的ISO

##### 步骤三：

- 将ISO进行装载
- 使用[Dism++](https://www.chuyu.me)将镜像写入SD卡，选择恢复功能->系统还原，之后选择ISO文件里面的`install.wim`和SD卡NTFS格式的目录

##### 步骤四：

- 下载UEFI：[路径1](https://github.com/andreiw/RaspberryPiPkg/archive/master.zip) ([路径2](https://github.com/driver1998/bsp)) 

- 找到`RaspberryPiPkg\Binary\prebuilt\2018May22-GCC49\RELEASE` 将其拷贝到FAT32盘

- 使用管理员身份打开命令提示符，输入以下命令（FAT32盘符是Y，NTFS是X）

  `bcdboot X:\Windows /s Y: /f UEFI /l zh-cn`

  ```bcdedit /store Y:\efi\microsoft\boot\bcd /set {Default} testsigning on ```

  ```bcdedit /store Y:\efi\microsoft\boot\bcd /set {Default} nointegritychecks on```

##### 步骤五：

- 下载[驱动](https://github.com/RpiWin10/Drivers/archive/master.zip)，解压放到一个路径例如`C:\drivers`
- 运行```dism /image:X: /add-driver /driver:C:\driver /forceunsigned```

##### 步骤六：

- 设置cpu频率，打开FAT32盘下面的`config.txt` 添加`arm_freq=1200`频率为1200或者800

- 通电，按住`ESC`键，进入UEFI设置，选择`DeviceManager –> Raspberry Pi Configuration`

- 在`HypDxe Configuration`更改为` Boot in EL1`

- [可选] `Chipset Configuration`里将`CPU Clock`项调为`Max`

# English version  
### How to install Windows10 arm version for Raspberry 3B

## ⚠️[Warning] Something about install win10 arm into raspberry pie, but it's not verified

#### Requirements：

- Raspberry 3B
- 16G SD card or bigger
- Windows 10 arm ISO
- microSD card converter

##### Step 1:

- Formatting your SD card
- Use [DiskGenius](http://www.diskgenius.cn/download.php) to divide your SD card to two zones, first one disk size is about 100MB format is FAT32, than using the remaining capacity to create another partition,  format is NTFS (using MBR format for partition table)

##### Step 2:

- Go to [UUP dump](http://mdluup.ct8.pl/index.php) and download the image downloader. Click the `known build` at the Home page and choose an image that is Arm64 version, alfter select the Language and Edition click the `Dowdload using aria2 and then convert` to download the tool. You will receive a compress package.
- Than unzip the package, using the `aria2_download.cmd` tool to download the image, alfter that you will get an ISO file. 

##### Step 3:

- Load the ISO file
- Write `install.wim` in ISO file to SD card using  [Dism++](https://www.chuyu.me) 

#####  Step 4:

- Download teh UEFI boot: [Path 1](https://github.com/andreiw/RaspberryPiPkg/archive/master.zip) ([Path 2](https://github.com/driver1998/bsp)) 

- Go to the path `RaspberryPiPkg\Binary\prebuilt\2018May22-GCC49\RELEASE` and copy all the file to the FAT32 partition

- Open the CMD as Administrator, using the cmd line below(`Y` is the path of FAT32 partition, and `X` is NTFS partition)

  `bcdboot X:\Windows /s Y: /f UEFI /l en-us`

  ```bcdedit /store Y:\efi\microsoft\boot\bcd /set {Default} testsigning on ```

  ```bcdedit /store Y:\efi\microsoft\boot\bcd /set {Default} nointegritychecks on```

##### Step 5:

- Download the [Drivers](https://github.com/RpiWin10/Drivers/archive/master.zip), unzip into path like `C:\drivers`
- Than run ```dism /image:X: /add-driver /driver:C:\drivers /forceunsigned```

##### Step 6:

- Set the cpu frequent, open the `config.txt`  on the FAT32 partition, add this `arm_freq=1200`, the cpu frequent can be set as 1200 or 800
- Plugin the power, press the `ESC` button entering the UEFI setting, chose `DeviceManager –> Raspberry Pi Configuration`
- Change the `HypDxe Configuration` to ` Boot in EL1`
- [Optional]Change the `CPU Clock` to `Max` in the `Chipset Configuration`
