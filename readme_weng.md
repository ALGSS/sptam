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

## ros安装

## SuiteSparse
sudo apt-get install libsuitesparse-dev

## ...


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

