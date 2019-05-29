# 在ubuntu18.04上安装OpenCV3.4.6
---  
## 前言
>这是一篇充满泪水的安装过程记录，希望这篇记录可以对大家有所帮助。  
---  
## OpenCV下载
>[下载链接](https://opencv.org/releases.html)，笔者安装的是3.4.6版本，大家可以根据需要自行选择。  
---  
## 安装准备
老习惯，先更新下自己的软件列表与软件。  
>sudo apt-get update  
>sudo apt-get upgrade  
---  
## 安装gcc与g++开发环境  
先查看下是否已经安装了gcc与g++。 
>gcc --version  
>g++ --version  
如果未安装。  
>sudo apt-get install build-essential  
然后查看gcc版本，安装与gcc统一版本的g++。（这里以4.4版本为例，具体情况具体分析）  
>gcc --version  
>sudo apt-get install g++-4.4  
---  
## 安装构建OpenCV的相关工具  
>sudo apt-get install cmake git pkg-config   
---  
## 安装常用图像工具包  
>sudo apt-get install libjpeg8-dev   
>sudo apt-get install libtiff5-dev   
>sudo apt-get install libjasper-dev   
>sudo apt-get install libpng12-dev   
> >特别注意！特别注意！特别注意！  
> >在执行sudo apt-get install libjasper-dev时，可能会报错：errorE: unable to locate libjasper-dev。  
> >解决方法：  
> > >sudo add-apt-repository "deb http://security.ubuntu.com/ubuntu xenial-security main"  
> > >sudo apt update  
> > >sudo apt install libjasper1 libjasper-dev   
---   
## 安装视频I/O包   
>sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev  
---  
## 安装gtk2.0  
>sudo apt-get install libgtk2.0-dev  
---  
## 安装优化函数包  
>sudo apt-get install libatlas-base-dev gfortran  
---  
## 解压opencv-3.4.2，并在解压好的文件夹内新建Release文件夹。  
---  
## CMake配置编译  
在Release文件夹内打开终端  
>cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D BUILD_TIFF=ON ..   
> >特别注意！特别注意！特别注意！  
> >BUILD_TIFF=ON这个设置很重要。笔者一开始没有设置这个参数，导致在make编译过程中，进行到Linking CXX executable时疯狂抛/usr/lib/libopencv_highgui.so.2.4: undefined reference to TIFFRGBAImageOK@LIBTIFF_4.0'等类似错误，设置该参数后便会自动编译libtiff4来消除错误。  
---  
## Make编译  
>sudo make  
---  
## 安装  
>sudo make install  
---  
## 环境配置添加库路径  
>sudo gedit /etc/ld.so.conf.d/opencv.conf   
>#打开后可能是空文件，在文件内容最后添加  
>/usr/local/lib  
---  
## 更新系统库  
>sudo ldconfig  
---  
## 配置bash，执行如下命令  
>sudo gedit /etc/bash.bashrc  
>#在末尾添加以下内容  
>PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig/  
>export PKG_CONFIG_PATH  
---  
## 保存退出，然后执行如下命令使得配置生效  
>source /etc/bash.bashrc  
>#激活配置然后更新database  
>sudo updatedb   
---  
### 在ubuntu18.04上安装与配置OpenCV3.4.6的过程到此结束。  
---   
## 测试   
* 查看OpenCV的安装库   
>>pkg-config opencv --libs   
* 查看OpenCV安装版本   
>>pkg-config opencv --modversion   
* 运行cpp代码测试  
* 创建项目目录  
>>mkdir opencv_example  
* 将项目代码放入文件夹  
* 在项目目录下创建cmake编译文件   
>>vim CMakeLists.txt  
* 在该文件中添加以下内容    
>>cmake_minimum_required(VERSION 2.8)  
>>project(opencv_example)
>>find_package(OpenCV REQUIRED)  
>>add_executable(opencv_example opencv_example.cpp)  
>>target_link_libraries(opencv_example ${OpenCV_LIBS})   
* 此时在该目录下存在以下的文件： CMakeLists.txt,opencv_example.cpp 。   
* 编译    
>>cmake .   
>>make   
* 运行   
>>./opencv_exampel   
#### 不出意外，完美运行。。。。。。 

---   
# END  
