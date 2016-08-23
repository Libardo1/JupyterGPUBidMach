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
Inside bidmach:

```
export SPARK_HOME='/home/shlomo/dev/spark/'
export PYTHONPATH=$SPARK_HOME/python/:$PYTHONPATH:$SPARK_HOME/python/lib/py4j-0.9-src.zip
export XDG_RUNTIME_DIR=""
```

## Install Spark 1.6
### Configuration





