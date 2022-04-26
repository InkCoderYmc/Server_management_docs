# **CUDA切换教程**

为满足多版本Pytorch、TensorFlow的现实需求，现丰富服务器CUDA版本，使用方式如下。

## **1、切换CUDA的shell命令**

```
export CUDA_version=10.0
export PATH=/usr/local/cuda-$CUDA_version/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-$CUDA_version/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

cuda一共有9.0，10.0，10.1，11.1，11.3可以切换。系统/etc/profile默认设置的是10.1的版本

## **2、临时设置方式（可在screen里方便使用）**

就直接在命令行里粘贴上述命令，回车即可。

注释：

1. 第一行复制时，不要带上结尾的回车，不然自动换行就没法改命令里的CUDA版本了。
2. 第二三行绑定了前面的变量，可以直接贴。

## **3、永久设置方式（按照顺序操作即可）**

在~/.bashrc的最后，粘贴上述命令，保存退出，参考命令如下

| 命令                               | 含义                               |
| ---------------------------------- | ---------------------------------- |
| vim ~/.bashrc                      | 进入用户个人配置文件               |
| :$                                 | 转到最后一行                       |
| i                                  | 进入编辑模式                       |
| 粘贴上述命令，已有的则修改CUDA版本 | 根据需要修改CUDA_version的数值     |
| esc接:wq                           | 保存并推出vim                      |
| source ~/.bashrc                   | 重载新环境变量（不做这一步便无效） |

## **4、检查CUDA及CUDNN版本号**

CUDA：

```
nvcc -V
```

CUDNN：

```
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
```

注释：/cuda/要换成对应的版本号，例如/cuda-9.0/

## **5、Pytorch、TensorFlow安装**

pip推荐腾讯源：在~/.pip/pip.conf配置，参考[如何使用腾讯云镜像源加速pip | 时鹏亮的blog (shipengliang.com)](https://shipengliang.com/software-exp/如何使用腾讯云镜像源加速pip.html)

conda推荐北外源：在~/.condarc配置，参考[anaconda | 镜像站使用帮助 | 北京外国语大学开源软件镜像站 | BFSU Open Source Mirror](https://mirrors.bfsu.edu.cn/help/anaconda/)

新注解：北外好像也不大行了，换上海交通大学的镜像吧，参考[已整理：anaconda更换国内镜像源配置_styuan/dy的博客-CSDN博客_anaconda配置国内源](https://blog.csdn.net/yst990102/article/details/106730382)

我这边home目录下的.condarc配置文件长这样：

```
default_channels:
  - https://anaconda.mirrors.sjtug.sjtu.edu.cn/pkgs/r
  - https://anaconda.mirrors.sjtug.sjtu.edu.cn/pkgs/main
custom_channels:
  conda-forge: https://anaconda.mirrors.sjtug.sjtu.edu.cn/cloud/
  pytorch: https://anaconda.mirrors.sjtug.sjtu.edu.cn/cloud/
channels:
  - defaults
envs_dirs:
  - /data/zhangjinhao/envs
```

方式：直接把大框里的几行文本，贴进去，再保存就行。

注：torch有自己的强制默认源，用官网命令行里-f附带的就行，下载速度也很快



### **5.1 Pytorch**

最新版Pytorch：[PyTorch](https://pytorch.org/)

历史版本Pytorch：[Previous PyTorch Versions | PyTorch](https://pytorch.org/get-started/previous-versions/)，选Linux and Windows下面的，conda或pip安装都可以，推荐用pip

注：**Pytorch**要严格对应CUDA版本，CUDA的整数、小数版本都要对上



### **5.2 TensorFlow**

安装命令：pip install tensorflow-gpu==版本号

注：从**TensorFlow**的实际情况看，CUDA的A.B版本，只要A相同就可以了

例如，需要10.0版本的CUDA，10.1和10.0好像都可以跑TensorFlow-GPU

新批注：tensorflow1貌似只认10.0，不认10.1

**tensorflow也**要严格对应CUDA版本，CUDA的整数、小数版本都要对上



**TensorFlow-GPU与CUDA、CUDNN适配表-Linux版本**

[从源代码构建  | TensorFlow (google.cn)](https://tensorflow.google.cn/install/source#linux)



| 版本                  | Python 版本  | cuDNN | CUDA |
| --------------------- | ------------ | ----- | ---- |
| tensorflow-2.5.0      | 3.6-3.9      | 8.1   | 11.2 |
| tensorflow-2.4.0      | 3.6-3.8      | 8.0   | 11.0 |
| tensorflow-2.3.0      | 3.5-3.8      | 7.6   | 10.1 |
| tensorflow-2.2.0      | 3.5-3.8      | 7.6   | 10.1 |
| tensorflow-2.1.0      | 2.7、3.5-3.7 | 7.6   | 10.1 |
| tensorflow-2.0.0      | 2.7、3.3-3.7 | 7.4   | 10.0 |
| tensorflow_gpu-1.15.0 | 2.7、3.3-3.7 | 7.4   | 10.0 |
| tensorflow_gpu-1.14.0 | 2.7、3.3-3.7 | 7.4   | 10.0 |
| tensorflow_gpu-1.13.1 | 2.7、3.3-3.7 | 7.4   | 10.0 |
| tensorflow_gpu-1.12.0 | 2.7、3.3-3.6 | 7     | 9    |
| tensorflow_gpu-1.11.0 | 2.7、3.3-3.6 | 7     | 9    |
| tensorflow_gpu-1.10.0 | 2.7、3.3-3.6 | 7     | 9    |
| tensorflow_gpu-1.9.0  | 2.7、3.3-3.6 | 7     | 9    |
| tensorflow_gpu-1.8.0  | 2.7、3.3-3.6 | 7     | 9    |
| tensorflow_gpu-1.7.0  | 2.7、3.3-3.6 | 7     | 9    |
| tensorflow_gpu-1.6.0  | 2.7、3.3-3.6 | 7     | 9    |
| tensorflow_gpu-1.5.0  | 2.7、3.3-3.6 | 7     | 9    |
| tensorflow_gpu-1.4.0  | 2.7、3.3-3.6 | 6     | 8    |
| tensorflow_gpu-1.3.0  | 2.7、3.3-3.6 | 6     | 8    |
| tensorflow_gpu-1.2.0  | 2.7、3.3-3.6 | 5.1   | 8    |
| tensorflow_gpu-1.1.0  | 2.7、3.3-3.6 | 5.1   | 8    |
| tensorflow_gpu-1.0.0  | 2.7、3.3-3.6 | 5.1   | 8    |