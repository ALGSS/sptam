代码位置：
https://github.com/lrse/sptam

参考双目的一个实现：
https://github.com/uoip/stereo_ptam






# 编译

参考说明文档 https://github.com/lrse/sptam
```bash
catkin_make --pkg sptam -DCMAKE_BUILD_TYPE=RelWithDebInfo -DSINGLE_THREAD=OFF -DSHOW_TRACKED_FRAMES=ON -DSHOW_PROFILING=ON -DPARALLELIZE=ON
```

To activate Loop Closing capabilities (requires DBoW2 and OpenGV dependencies).

```bash
catkin_make --pkg sptam -DCMAKE_BUILD_TYPE=RelWithDebInfo -DUSE_LOOPCLOSURE=ON -DSINGLE_THREAD=OFF -DSHOW_TRACKED_FRAMES=ON -DPARALLELIZE=ON -DSHOW_PROFILING=ON 
```

## g2o
```bash
git clone git@github.com:RainerKuemmerle/g2o.git

# 选择性操作
# Tested until commit 4b9c2f5b68d14ad479457b18c5a2a0bce1541a90
# git checkout 4b9c2f5b68d14ad479457b18c5a2a0bce1541a90


mkdir build && cd build
cmake ..
make -j6
sudo make install
```


## ros安装
...

## SuiteSparse
sudo apt-get install libsuitesparse-dev

## others （Loop Closure dependencies）

* git submodule update --init --recursive
* DLib
* DBoW2
* DLoopDetector
* OpenGV


## 问题1
sptam/src/sptam/types_sba_extension.hpp:43:84: error: ‘VertexSBAPointXYZ’ was not declared in this scope; did you mean ‘VertexPointXYZ’?

解决：
新版g2o库中不再存在原先的VertexSBAPointXYZ，将所有报错的地方改为VertexPointXYZ即可。

ref
[SLAM十四讲ch7代码调整（undefined reference to symbol）
](https://blog.csdn.net/weixin_43747622/article/details/117907852)


## 问题2

sptam/MapPoint.hpp:51:17: error: ‘cv’ is not a namespace-name

分析
1、错误原因很多，根据经验，如果出现is not a class or namespace name，就是没有正确包含声明了某个类的头文件.
2、也许实际上包含。如果仍然出现这个提示，可能是从网页或者其他途径复制了代码，代码中包含有VC编译器不能识别的字符。
解决方法：将所有复制过来的代码中的空格和回车分别选中并自己重新输入一遍。

REF
https://zhidao.baidu.com/question/201840608.html

## 问题3

DLibConfig.cmake

解决
安装 DLib


## 问题4

/usr/local/include/DVision/BRIEF256.h:207:11: error: ‘cvtColor’ is not a member of ‘cv’

```html
[ 88%] Building CXX object sptam/src/sptam/CMakeFiles/sptam.dir/loopclosing/detectors/FBRISK.cpp.o
In file included from /usr/local/include/DVision/DVision.h:42,
                 from /home/weng/DEV/Projs/ptam/sptam_ws/src/sptam/src/sptam/loopclosing/detectors/FBRISK.cpp:20:
/usr/local/include/DVision/BRIEF256.h: In member function ‘void DVision::BRIEF_t<Bits>::compute(const cv::Mat&, const std::vector<cv::KeyPoint>&, std::vector<std::bitset<Bits> >&, bool) const’:
/usr/local/include/DVision/BRIEF256.h:207:11: error: ‘cvtColor’ is not a member of ‘cv’
  207 |       cv::cvtColor(image, aux, cv::COLOR_RGB2GRAY);
      |           ^~~~~~~~
/usr/local/include/DVision/BRIEF256.h:207:36: error: ‘COLOR_RGB2GRAY’ is not a member of ‘cv’
  207 |       cv::cvtColor(image, aux, cv::COLOR_RGB2GRAY);
      |                                    ^~~~~~~~~~~~~~
/usr/local/include/DVision/BRIEF256.h:214:9: error: ‘GaussianBlur’ is not a member of ‘cv’
  214 |     cv::GaussianBlur(aux, im, ksize, sigma, sigma);
      |         ^~~~~~~~~~~~
make[2]: *** [sptam/src/sptam/CMakeFiles/sptam.dir/build.make:492：sptam/src/sptam/CMakeFiles/sptam.dir/loopclosing/detectors/FBRISK.cpp.o] 错误 1
make[2]: *** 正在等待未完成的任务....

```

解决
按照 DLib 的安装目录 build/install_manifest.txt，进行删除安装的文件。
```bash
# 危险操作，注意核对目录
sudo rm -rf /usr/local/lib/libDLib.so
sudo rm -rf /usr/local/include/DUtils/
sudo rm -rf /usr/local/include/DUtilsCV/
sudo rm -rf /usr/local/include/DVision/
sudo rm -rf /usr/local/include/DLib/
sudo rm -rf /usr/local/lib/cmake/DLib/
```
然后下载一个支持opencv4的DLib进行安装。








备注1：

```bash
# opencv3中：
cvtColor(input, output, CV_BGR2GRAY；
# 而在opencv4中变为
cvtColor(input, output, COLOR_COLOR_RGB2GRAY);
```


备注2：

误删 /usr/local/lib/cmake 恢复

方法1：

https://blog.csdn.net/qq_36355662/article/details/61919474

方法2

直接，重新安装 `sudo make install` 之前的程序，如果不多的话。

* x  DBoW2
* x  opengv
* x  DLoopDetector
* x  g2o
* python2.7
* python 3.8
* x  opencv 3.2
* x  ceres


REF

https://blog.csdn.net/weixin_43013458/article/details/95369695

