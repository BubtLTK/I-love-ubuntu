# 在ubuntu18.04上搭建Halide环境
---  
## 前言
>这又是一篇充满泪水的安装过程记录，希望这篇记录可以对大家有所帮助。  
---  
## LLVM下载安装
* LLVM的安装方法有很多，就不一一阐述了，笔者采用的方法是配置从LLVM官网下载预编译好的包[下载链接](http://releases.llvm.org/download.html)，大家可以根据需要自行选择版本，至少8.0；笔者的系统版本是ubuntu18.04，所以选择对应的Ubuntu18.04包（点击Pre-Built Binaries下的Ubuntu 18.04 (.sig)进行下载）。  
* 下载之后解压到一个目录下，解压后的文件夹下包含bin,include,lib,libexec,share文件夹，将bin文件夹添加进环境变量。   
* 测试LLVM是否已经配置成功：    
  `llvm-config --version`    
  `clang --version`     

---  
## Halide下载安装
[下载链接](https://github.com/halide/Halide)--安装前最好（必须）先浏览一遍README（不过按照步骤来可能还是会遇到caodan的问题）。笔者分别用了两种方法进行编译安装的尝试。机子不好的同学请不要随便用make -j！机子不好的同学请不要随便用make -j！机子不好的同学请不要随便用make -j！这是并行编译，按道理讲速度确实很快，但是编译到一半会让机子死机（笔者死机两次，获得重装系统大奖一枚）！    
* 直接make（失败）   
    
跑[程序](https://blog.csdn.net/luzhanbo207/article/details/78655484)测试时报错，找不到动态链接共享库（.so文件）。（笔者只在编译安装目录下找到了静态库文件（.a文件）...可能我瞎了？？？）  
   
有精力的小伙伴可以尝试下创建动态链接共享库文件。（反正我没有QAQ）   
  
* 先cmake再make（成功）  
    
make的过程中可能会出现很迷的错误，会在进度到90%多的时候给你报错，比如"对‘cblas_scopy(int, float const*, int, float*, int)’未定义的引用"等系列错误！！！查阅资料发现这个问题是由于我的Ubuntu同时安装了blas和atlas导致的，atlas library会被当作blas library来用，好的，卸载atlas library。    
   
  `sudo dpkg -l | grep atlas`    
    
将上条命令执行结果中的东西全remove掉，再次make。
    
然后又报错：cannot find -lcblas...解决方案：安装libatlas-base-dev包（这不是我之前remove掉的嘛？？？）；果不其然，install后又开始报之前的错的（笔者试过删blas留atlas，但是没用）。    
    
于是笔者一气之下删了atlas的同时又删了Halide的项目源码，重新clone项目源码。   
    
再次cmake，再次make，再次cannot find -lcblas，再次`sudo apt-get install libatlas-base-dev`，再次make，然后就...100%了（？？？这就是佛系的力量？？？）。   
   
但是跑程序测试的时候又TM的报错了（/usr/bin/x86_64-linux-gnu-ld: 找不到 -lHalide），就知道没这么简单...但是不慌！虽然这个也是找不到动态库的问题，但是这个和直接make报错的不一样，在本次make目录下的lib目录里，存在着libHalide.so文件，这不就是我们要找的动态链接共享库嘛！二话不说，直接将该文件cp到/usr/lib目录下（懒）。再次测试程序，成功！！！  
   
---  
## PS：征服了Ubuntu18.04，再去征服一下实验室的Ubuntu16.04，而且实验室的系统是新装的，应该相对容易些。^_^      
果然，遇到的问题都是找不到库之类的（/usr/bin/ld：找不到 -lXXXX)，只要安装对应的依赖库即可；笔者对这次的问题以及解决过程做一下记录。   
> * /usr/bin/ld：找不到 -lz    
 解决命令：sudo apt-get install zlib1g-dev    
> * /usr/bin/ld：找不到 -ltinfo   
 解决命令：sudo apt-get install libtinfo-dev     
> * /usr/bin/ld：找不到 -lxml2     
 解决命令：sudo apt-get install libxml2-dev      

---  
### 虽然篇幅不长，但这环境的搭建还是卡了笔者一整天的时间，毕竟Halide在国内的相关教程较少，笔者只能自己摸索。（留下英语菜鸟的泪水）  
#### PPS:佛系的力量果然强大～     
---   
# END  
