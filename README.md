# JupyterGPUBidMach
Integrate BidMach with Jupyter + Scala + GPU + CUDA.
A collection of scripts to utilize Bidmach on Jupyter.

## Target OS:
Installed on Ubuntu 14 64BIT, but should work on recent releases too. 

* shlomo@anycub01:~/Downloads/GpuTest_Linux_x64_0.7.0$ uname -a
* Linux anycub01 4.2.0-35-generic #40~14.04.1-Ubuntu SMP Fri Mar 18 16:37:35 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux

## Target GPU:
NVIDIA GPU family. Mine is:

## Install Anaconda Jupyter 

### Configuration
Inside jupyter_notebook_config.py:


## Install BidMach 
### Configuration
Inside 'bidmach':

* export SPARK_HOME='/home/shlomo/dev/spark/'
* export PYTHONPATH=$SPARK_HOME/python/:$PYTHONPATH:$SPARK_HOME/python/lib/py4j-0.9-src.zip
* export XDG_RUNTIME_DIR=""

## Install Spark 1.6
### Configuration





