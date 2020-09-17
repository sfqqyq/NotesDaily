> 在ubuntu上安装python是一个非常方便的操作

## 主要步骤
- 下载python安装包
- 在/opt目录下解压
- 执行 ./configura
- 执行make && make install  (中间可能会出错，需要安装对应的依赖包，查看需要什么安装包，gcc之类的)
- 将安装好的python配置到环境变量中


## 一、下载安装包
在官网下载选择.taz结尾的就好
  url： https://www.python.org/ftp/python/3.8.5/

## 二、解压压缩包
~~~
  cd /opt  # 进入目录
  tar -zxvf Python-3.8.5.tgz # 解压压缩包
~~~

## 三、配置安装路径
~~~
./configure --prefix=/opt/python3.8
~~~

## 四、安装
~~~
make && make install # 在解压好的文件夹中执行
~~~

## 五、配置环境变量
~~~
修改~/.bashrc
添加：export PYTHONPATH="/yourpath/:$PYTHONPATH"
执行source ~/.bashrc使修改立即生效
~~~

