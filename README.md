# Spark Recipes


## Installing Jupyter in a local virtualenv configured for Spark
The Scala REPL and Spark Shell are not especially user friendly.
Fortunately Jupyter Notebooks provides a much nicer experience.  
The recipe belows uses virtualenv to avoid installing over any other
existing python libraries.

### Spark Setup ###
* To install Spark locally download the [latest release of Spark](https://spark.apache.org/downloads.html) and choose package type `Pre-built for Hadoop 2.6 and later`.
* Unpack the contents of the download.  An easy place to put it is `$HOME/vendor`
* Set your `SPARK_HOME` environment variable:
```
export SPARK_HOME=~/vendor/spark-1.6.1-bin-hadoop2.6
```

### Setup virtualenv 
```
virtualenv sparksandbox
source bin/activate
```

### Install Jupyter and Toree
[Apache Toree](https://toree.incubator.apache.org/) serves as middleware for communicating with a Spark cluster.  Its the little bit of ``magics`` that 
hooks Spark into the Juptyer/ipython environment.

```
pip install jupyter
pip install --pre toree
export PYTHONPATH=$SPARK_HOME/python:$SPARK_HOME/python/build:$PYTHONPATH
export PYTHONPATH=$SPARK_HOME/python/lib/py4j-0.9-src.zip:$PYTHONPATH
jupyter toree install \
    --spark_home=$SPARK_HOME \
    --spark_opts='--master=local[4]' \
    --interpreters=PySpark,SQL,Scala
```
