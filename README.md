# JupyterGPUBidMach
Integrate BidMach with Jupyter + Scala + GPU + CUDA.
A collection of scripts to utilize Bidmach on Jupyter.

## Target OS:
Installed on Ubuntu 14 64BIT, but should work on recent releases too. 

* shlomo@anycub01:~/Downloads/GpuTest_Linux_x64_0.7.0$ uname -a
* Linux anycub01 4.2.0-35-generic #40~14.04.1-Ubuntu SMP Fri Mar 18 16:37:35 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux

## Target GPU:
NVIDIA GPU family. Mine is:
![My GPU](gpu.png)

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





