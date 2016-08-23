# JupyterGPUBidMach
Integrate BidMach with Jupyter + Scala + GPU + CUDA.
A collection of scripts to utilize Bidmach on Jupyter.

## Target OS:
Installed on Ubuntu 14 64BIT, but should work on recent releases too. 

```
shlomo@xxx$ uname -a
Linux xxx 4.2.0-35-generic #40~14.04.1-Ubuntu SMP Fri Mar 18 16:37:35 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux
```

## Target GPU:
NVIDIA GPU family. Mine is:

```
shlomo@xxx:$ lspci  -v -s  $(lspci | grep VGA | cut -d" " -f 1)
01:00.0 VGA compatible controller: NVIDIA Corporation GM204 [GeForce GTX 970] (rev a1) (prog-if 00 [VGA controller])
	Subsystem: Gigabyte Technology Co., Ltd Device 367a
	Flags: bus master, fast devsel, latency 0, IRQ 134
	Memory at f6000000 (32-bit, non-prefetchable) [size=16M]
	Memory at e0000000 (64-bit, prefetchable) [size=256M]
	Memory at f0000000 (64-bit, prefetchable) [size=32M]
	I/O ports at e000 [size=128]
	[virtual] Expansion ROM at f7000000 [disabled] [size=512K]
	Capabilities: <access denied>
	Kernel driver in use: nvidia
```
## Install CUDA

```
#!/usr/bin/env bash

if [ "$(whoami)" == "root" ]; then
  echo "running as root, please run as user you want to have stuff installed as"
  exit 1
fi

sudo apt-get update -y
sudo apt-get install -y git wget linux-image-generic build-essential unzip

sudo apt-get install  libglew-dev libcheese7 libcheese-gtk23 libclutter-gst-2.0-0 libcogl15 libclutter-gtk-1.0-0 libclutter-1.0-0  xserver-xorg-input-all


cd /tmp
wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1404/x86_64/cuda-repo-ubuntu1404_7.0-28_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1404_7.0-28_amd64.deb

echo -e "\nexport CUDA_HOME=/usr/local/cuda\nexport CUDA_ROOT=/usr/local/cuda" >> ~/.bashrc
echo -e "\nexport PATH=/usr/local/cuda/bin:\$PATH\nexport LD_LIBRARY_PATH=/usr/local/cuda/lib64:\$LD_LIBRARY_PATH" >> ~/.bashrc

sudo apt-get update
sudo apt-get install cuda-toolkit-7-0
sudo apt-get install nvidia-modprobe
pip install pycuda
```


## Install Anaconda Jupyter 

### Configuration
Inside jupyter_notebook_config.py:

```
c = get_config()
 
c.NotebookApp.ip = '192.168.10.150'
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8880 # or whatever you want; be aware of conflicts with CDH

import os
import sys

spark_home = os.environ.get('SPARK_HOME', None)
if not spark_home:
    raise ValueError('SPARK_HOME environment variable is not set')

os.environ['PYSPARK_SUBMIT_ARGS']='--packages com.databricks:spark-csv_2.11:1.4.0 --master local[*] --driver-memory 12g --num-executors 2 --executor-cores 2 --executor-memory 12g  pyspark-shell'

#Dummy
os.environ['HADOOP_CONF_DIR']='/home/shlomo/dev/'

sys.path.insert(0, os.path.join(spark_home, 'python'))
sys.path.insert(0, os.path.join(spark_home, 'python/lib/py4j-0.9-src.zip'))
```

## Install BidMach 
### Configuration
Install the Binary BidMach distribution but ... replace 'bidmach' with the 'bidmach' binary provided with this repo. Make sure to change the variables to reflect your environment. 

Inside bidmach:

```
export SPARK_HOME='/home/shlomo/dev/spark/'
export PYTHONPATH=$SPARK_HOME/python/:$PYTHONPATH:$SPARK_HOME/python/lib/py4j-0.9-src.zip
export XDG_RUNTIME_DIR=""
```

## Install Spark 1.6
### Configuration

## Start Jupyter 

```
total 1276
drwxrwxr-x 10 shlomo shlomo    4096 אוג 21 08:42 .
drwxrwxr-x 13 shlomo shlomo    4096 אוג 23 08:16 ..
-rwxrwxr-x  1 shlomo shlomo    3262 אוג 17 17:17 bidmach
-rwxr-xr-x  1 shlomo shlomo    2832 מאי 20  2015 bidmach2
-rwxr-xr-x  1 shlomo shlomo    2832 מאי 20  2015 bidmach2~
-rw-rw-r--  1 shlomo shlomo   63695 אוג 21 08:42 BIDMach_basic_classification.ipynb
-rwxrwxr-x  1 shlomo shlomo     896 מאי 20  2015 bidmach.cmd
-rwxr-xr-x  1 shlomo shlomo    1502 מאי 20  2015 bidmach_full
-rw-rw-r--  1 shlomo shlomo    1749 מאי 20  2015 bidmach_full~
-rw-rw-r--  1 shlomo shlomo 1135913 מאי 20  2015 BIDMach.jar
-rwxrwxr-x  1 shlomo shlomo    1272 מאי 20  2015 build.sbt
drwxrwxr-x  2 shlomo shlomo    4096 מאי 20  2015 cbin
-rwxrwxr-x  1 shlomo shlomo    1552 מאי 20  2015 Copyright.txt
drwxrwxr-x  4 shlomo shlomo    4096 מאי 20  2015 data
-rwxrwxr-x  1 shlomo shlomo     723 מאי 20  2015 getlibs.sh
-rw-rw-r--  1 shlomo shlomo     509 מאי 20  2015 getnativepath.class
-rw-rw-r--  1 shlomo shlomo     176 מאי 20  2015 getnativepath.java
drwxr-xr-x  2 shlomo shlomo    4096 אוג 17 17:11 .ipynb_checkpoints
-rw-rw-r--  1 shlomo shlomo     157 אוג 17 16:32 java_native_path.txt
drwxrwxr-x  4 shlomo shlomo    4096 מאי 20  2015 jni
drwxrwxr-x  2 shlomo shlomo    4096 מאי 20  2015 lib
-rwxrwxr-x  1 shlomo shlomo     817 מאי 20  2015 README.md
-rwxrwxr-x  1 shlomo shlomo     148 מאי 20  2015 sbt
drwxrwxr-x  3 shlomo shlomo    4096 מאי 20  2015 scripts
-rwxrwxr-x  1 shlomo shlomo      23 מאי 20  2015 shortpath.bat
drwxrwxr-x  3 shlomo shlomo    4096 מאי 20  2015 src
drwxrwxr-x  3 shlomo shlomo    4096 אוג 21 16:27 tutorials

shlomo@xxx:~/dev/new-db/BIDMach64$ ./bidmach notebook
```




