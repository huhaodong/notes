# ubuntu16.04 64bit cuda9.0 cudnn7.1 源码安装tensorflow

## 1.安装java8
sudo add-apt-repository ppa:webupd8team/java //添加java源

sudo apt-get update

sudo apt-get install oracle-java8-installer

java -version //test

---

## 2.安装bazel

echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list

curl https://bazel.build/bazel-release.pub.gpg | sudo apt-key add -

sudo apt-get update && sudo apt-get install bazel

sudo apt-get upgrade bazel

---

## 3.安装cuda 9.0 （保险）

如果安装的是8.0版本的cuda需要降低g++的版本。

cuda下载地址：https://developer.nvidia.com/cuda-downloads
选择ubuntu 16.04 的deb版本

sudo dpkg -i cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64.deb //此处的文件名要换成下载下来的文件名,这个操作会在/etc/apt/source.list.d/下面添加一个cuda开头的源文件，里面是cuda安装文件的地址。如果之前安装了其他的版本需要将其他版本的source文件删除掉。

sudo apt-key add /var/cuda-repo-9-0-local/7fa2af80.pub	//根据提示进行操作

sudo apt-get update

sudo apt-get install cuda //也可以带版本 cuda-9-0，安装好后会在/usr/local目录下出现cuda和cuda-9.0两个文件夹，两个里面是一样的。主要用cuda文件夹。

sudo gedit ~/.bashrc	//配置cuda环境变量

```

//在文件末尾添加一下变量

export LD_LIBRARY_PATH=”$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64”

export CUDA_HOME=/usr/local/cuda

exportPATH="$CUDA_HOME/bin:$PATH"

```

source ~/.bashrc	//应用环境变量

---

## 4.安装cudnn 7.1

下载地址：https://developer.nvidia.com/cudnn

下载cudnn需要登录nvidia帐号，登录后下载对应cuda版本的cudnn7.1。下载时选择linux通用的tgz文件，而不是具体的linux系统版本的deb文件。

tar -zxvf cudnn-9.0-linux-x64-v7.1.tgz	//解压后出现cuda文件夹

sudo cp cuda/include/cudnn.h /usr/local/cuda/include/

sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64/

sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*	//添加对文件的读权限

sudo ln -s /usr/local/cuda/lib64/libcudnn.so.7.1.3 /usr/local/cuda/lib64/libcudnn.so.7.1	//建立软链接以防止早不到7.1

---

## 5.切换显卡

由于安装cuda的时候会安装显卡驱动，或者是更新nvidia的源。因此先安装cuda可以很方便的安装或切换显卡驱动。

设置->软件和更新->附加驱动->选择nvidia驱动->应用更改->重启 //在点击附加驱动后需要等待一会才会出现可用的驱动选项。

重启后进入设置->详细信息，在图形条目中出现啦NVIDIA显卡的型号说明切换成功。

---

## 6.环境依赖安装

sudo apt-get update

sudo apt-get install python-numpy swig python-dev python-wheel python-enum34

pip install mock

---

## 7.tensorflow安装

git clone --recurse-submodules https://github.com/tensorflow/tensorflow //下载源码

cd tensorflow

./configure	//cuda选项选择y，然后配置cuda的路径，cuda版本，cudnn版本等。

```

//以下是我的配置选项可以参考一下

You have bazel 0.13.0 installed.
Please specify the location of python. [Default is /usr/bin/python]: 


Found possible Python library paths:
  /usr/local/lib/python2.7/dist-packages
  /usr/lib/python2.7/dist-packages
Please input the desired Python library path to use.  Default is [/usr/local/lib/python2.7/dist-packages]

Do you wish to build TensorFlow with jemalloc as malloc support? [Y/n]: y
jemalloc as malloc support will be enabled for TensorFlow.

Do you wish to build TensorFlow with Google Cloud Platform support? [Y/n]: n
No Google Cloud Platform support will be enabled for TensorFlow.

Do you wish to build TensorFlow with Hadoop File System support? [Y/n]: n
No Hadoop File System support will be enabled for TensorFlow.

Do you wish to build TensorFlow with Amazon S3 File System support? [Y/n]: n
No Amazon S3 File System support will be enabled for TensorFlow.

Do you wish to build TensorFlow with Apache Kafka Platform support? [Y/n]: n
No Apache Kafka Platform support will be enabled for TensorFlow.

Do you wish to build TensorFlow with XLA JIT support? [y/N]: n
No XLA JIT support will be enabled for TensorFlow.

Do you wish to build TensorFlow with GDR support? [y/N]: n
No GDR support will be enabled for TensorFlow.

Do you wish to build TensorFlow with VERBS support? [y/N]: n
No VERBS support will be enabled for TensorFlow.

Do you wish to build TensorFlow with OpenCL SYCL support? [y/N]: n
No OpenCL SYCL support will be enabled for TensorFlow.

Do you wish to build TensorFlow with CUDA support? [y/N]: y
CUDA support will be enabled for TensorFlow.

Please specify the CUDA SDK version you want to use, e.g. 7.0. [Leave empty to default to CUDA 9.0]: 9.0


Please specify the location where CUDA 9.0 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: 


Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 7.0]: 7.1


Please specify the location where cuDNN 7 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:


Do you wish to build TensorFlow with TensorRT support? [y/N]: n
No TensorRT support will be enabled for TensorFlow.

Please specify the NCCL version you want to use. [Leave empty to default to NCCL 1.3]: 


Please specify a list of comma-separated Cuda compute capabilities you want to build with.
You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
Please note that each additional compute capability significantly increases your build time and binary size. [Default is: 5.0]


Do you want to use clang as CUDA compiler? [y/N]: n
nvcc will be used as CUDA compiler.

Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/bin/gcc]: 


Do you wish to build TensorFlow with MPI support? [y/N]: n
No MPI support will be enabled for TensorFlow.

Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]: 


Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]: n
Not configuring the WORKSPACE for Android builds.

Preconfigured Bazel build configs. You can use any of the below by adding "--config=<>" to your build command. See tools/bazel.rc for more details.
	--config=mkl         	# Build with MKL support.
	--config=monolithic  	# Config for mostly static monolithic build.
Configuration finished

```

bazel build -c opt --config=cuda //tensorflow/tools/pip_package:build_pip_package	//编译tensorflow，编译需要很长的时间。其中可能会出现依赖问题，我这里出现的依赖问题已经在前面安装依赖中解决了。如果还有依赖问题需要一个一个安装上，然后编译。

bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg //运行后会在/tmp/tensorflow_pkg中生成安装包文件

sudo pip install /tmp/tensorflow_pkg/tensorflow-1.2.0rc2-cp27-cp27mu-linux_x86_64.whl	//使用pip命令安装生成的安装包文件。文件目录需要更改为你上一步设置的目录。

```python

//安装后测试

import tensorflow as tf

with tf.device('/cpu:0'):
        a=tf.constant([1.0,2.0,3.0],shape=[3],name='a')

        b=tf.constant([1.0,2.0,3.0],shape=[3],name='b')

with tf.device('/gpu:0'):
        c=a+b
sess=tf.Session(config=tf.ConfigProto(log_device_placement=True))

print sess.run(c)

```
