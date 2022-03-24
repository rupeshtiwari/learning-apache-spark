# Install spark step by step on your MAC machine

- [Install spark step by step on your MAC machine](#install-spark-step-by-step-on-your-mac-machine)
  - [Step 1: Install JAVA on your MAC](#step-1-install-java-on-your-mac)
  - [Step 2: Install Spark on your MAC](#step-2-install-spark-on-your-mac)
  - [Step 3: Installing `python3`](#step-3-installing-python3)
  - [Step 4: Running PySpark shell](#step-4-running-pyspark-shell)
  - [Step 5: Running small program with PySpark Shell](#step-5-running-small-program-with-pyspark-shell)
  - [Analyzing Spark Jobs using Spark Context Web UI](#analyzing-spark-jobs-using-spark-context-web-ui)
  - [Jupyter Notebook ( In local cluster and client mode )](#jupyter-notebook--in-local-cluster-and-client-mode-)
    - [How to use Spark using `Jupyter` Notebook?](#how-to-use-spark-using-jupyter-notebook)
      - [Step 1: Setting environment variable and starting notebook](#step-1-setting-environment-variable-and-starting-notebook)
      - [Let's do step 2: installing `findspark`](#lets-do-step-2-installing-findspark)
      - [Step 3 connecting spark with notebook shell](#step-3-connecting-spark-with-notebook-shell)
  - [Installing Multi-Node Spark Cluster at AWS](#installing-multi-node-spark-cluster-at-aws)
    - [Step 1: Creating EMR cluster at AWS cloud](#step-1-creating-emr-cluster-at-aws-cloud)
      - [What is Amazon EMR?](#what-is-amazon-emr)

You will able to install spark and also run spark shell and `pyspark` shell on your mac.

## Step 1: Install JAVA on your MAC

[Install JAVA steps on your mac machine](https://gist.github.com/rupeshtiwari/17ab26742618dc5d71b9dc5cfaff9bf9)

## Step 2: Install Spark on your MAC

- Go to apache spark site and download the latest version.
  https://spark.apache.org/downloads.html
- Create new folder `spark3`
- Move the tar file in `spark3` folder (newly created)
- `Untar` the folder with script
  `sudo tar -zxvf spark-3.2.1-bin-hadoop3.2.tgz`
- Set the Spark_home path to point to the `spark3` folder
  `export SPARK_HOME=~/spark3/spark-3.2.1-bin-hadoop3.2`
- Also put this script on startup command
  `sudo vim .zshrc`, Press “i” to edit, Press escape then :wq to save the file
  ![](https://i.imgur.com/pHJ8Ros.png)
- Open new terminal and check the spark home path
  - `echo $SPARK_HOME`
    ![](https://i.imgur.com/DP3x7iM.png)
- Next add the spark home bin path in your default \$PATH
  ![](https://i.imgur.com/P5oKrNk.png)
  - Here is the command to update \$path `export PATH=$PATH:$SPARK_HOME/bin`
    ![](https://i.imgur.com/y3Seres.png)
  - Place this path in your startup script as well
    ![](https://i.imgur.com/vIOAp8Q.png)
- Now you can start the spark shell type `spark-shell`
  ![](https://i.imgur.com/egFI12k.png)

## Step 3: Installing `python3`

If you already have `python3` then ignore this step. In order to check type `python3` on your terminal.

1. Install python3: `brew install python3`
2. Check `python3` installed: `python3`

   ![](https://i.imgur.com/3Q0KP1p.png)

3. Next setup `pyspark_python` environment variable to point python3:
   `export PYSPARK_PYTHON=python3`
4. Check the path: `echo $PYSPARK_PYTHON`
   ![](https://i.imgur.com/PhdrK4G.png)
5. Also put this script on your startup command file `.zshrc` file in my case.

   ![](https://i.imgur.com/J5EFm7c.png)

## Step 4: Running PySpark shell

Now run `pyspark` to see the spark shell in python.

![](https://i.imgur.com/kSPwkyN.png)

## Step 5: Running small program with PySpark Shell

You will learn about spark shell, local cluster, driver, executor and Spark Context UI.

| cluster | mode        | tool        |
| ------- | ----------- | ----------- |
| Local   | Client Mode | spark-shell |

```s
# Navigate to spark3 bin folder
cd ~/spark3/bin

# 1. create shell
pyspark
```

![](https://i.imgur.com/0J5htlK.png)

```s
# read and display json file with multiline formated json like I have in my example
df = spark.read.option("multiline","true").json("/Users/rupeshti/workdir/git-box/learning-apache-spark/src/test.json")
df.show()
```

👉 `option("multiline","true")` is important if you have JSON with multiple lines formated by prettier or any other formatter

![](https://i.imgur.com/5FNMXz9.png)

## Analyzing Spark Jobs using Spark Context Web UI

To monitor and investigate your spark application you can check spark context web UI.

- Go to url http://localhost:4040/jobs/
- Check Event Timeline Spark started and executed driver process
  ![](https://i.imgur.com/MJzJgmU.png)
- We are not seeing separate executer process, because we are in local cluster. Every thing is running in single JVM. JVM is a combination of driver and executer.
  ![](https://i.imgur.com/LdzomXg.png)
- When cluster created we did not pass number of thread so it took default number as `8` based on my laptop hardware resource available.
- Storage Memory it took maximum `434.4 MB`. This is sum of overall JVM.
- You can access this spark context UI till your spark shell is open. Once you quit spark shell you will loose the access to this UI.

## Jupyter Notebook ( In local cluster and client mode )

Data scientist use `Jupyter` Notebook to develop & explore application step by step. Spark programming in python requires you to have python on your machine.

| cluster | mode        | tool     |
| ------- | ----------- | -------- |
| Local   | Client Mode | Notebook |

If you install Anaconda environment, then you get python development environment also you will get spark support. You can [download](https://www.anaconda.com/products/individual) the community edition and install Anaconda. ANaconda comes with pre-configured `Jupyter` notebook.

### How to use Spark using `Jupyter` Notebook?

Notebook is a Shell based environment. You can type your code in shell and run it.

1. set `SPARK_HOME` environment variable
2. Install `findspark` package
3. Initialize `findspark`: the connection between Anaconda python environment and your spark installation

#### Step 1: Setting environment variable and starting notebook

![](https://i.imgur.com/a6tsF0B.png)

After installing Anaconda. Type `jupyter notebook` on terminal. You will see your browser will spin up at this URL on default browser: http://localhost:8888/tree
You shell will also be keep running on terminal. Now you have Jupyter notebook environment.

![](https://i.imgur.com/FkkofLe.png)

Go to desired folder and create a new python 3 notebook.

```python
import pyspark
from pyspark.sql import SparkSession

spark = SparkSession.builder.getOrCreate()
spark.read.option("multiline","true").json("/Users/rupeshti/workdir/git-box/learning-apache-spark/src/test.json").show()
```

![](https://i.imgur.com/QlCtaeD.png)

You get this error `ModuleNotFoundError: No module named 'pyspark'` because you have not connected the shell to spark.

#### Let's do step 2: installing `findspark`

```s
# 1. install pipx
brew install pipx

#2. install findspark
pip3 install findspark
```

![](https://i.imgur.com/er4r4dz.png)

#### Step 3 connecting spark with notebook shell

Below script will connect to spark.

```python
import findspark
findspark.init()
```

Final notebook code is:

```py
import findspark
findspark.init()

import pyspark
from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()
spark.read.option("multiline","true").json("/Users/rupeshti/workdir/git-box/learning-apache-spark/src/test.json").show()
```

![](https://i.imgur.com/ttPrHim.png)

## Installing Multi-Node Spark Cluster at AWS

At AWS, Amazon EMR (Elastic Map & Reduce)  service can be used to create `Hadoop` cluster with `spark`.

| cluster | mode        | tool                  |
| ------- | ----------- | --------------------- |
| Local   | Client Mode | spark-shell, Notebook |

This mode is used by data scientist for interactive exploration directly with production cluster. Most cases we use notebooks for web base interface and graph capability.  

### Step 1: Creating EMR cluster at AWS cloud

#### What is Amazon EMR? 

Amazon EMR is the industry-leading cloud big data platform for data processing, interactive analysis, and machine learning using open source framework such as Apache Spark, Apache Hive and Presto. 

Benefits of using Amazon EMR are:
1. You do not need to manage compute capacity or open-source applications that will save you time and money. 
2. Amazon EMR lets you to set up scaling rules to manage changing compute demand 
3. You can set up CloudWatch alerts to notify you of changes in your infrastructure and take actions immediately 
4. EMR has optimized runtime which speed up your analysis and save both time and money 
5. You can submit your workload to either EC2 or EKS using EMR 



- I am create cluster with 1 master and 3 worker nodes.
![](https://i.imgur.com/LaDEEwU.png)
- Use spark `version 2.4.8`
- We will use notebook so lets also take `Zeppelin 0.10.0`
  ![](https://i.imgur.com/SImxHJB.png)
- Create the cluster
  ![](https://i.imgur.com/qj0MMAv.png)
- Note you get 3 slave and 1 master EC2 instances created. 
- Go to security group of master node, add new rule, and allow all traffic from your IP address.
- SSH to Master instance. `ssh -i "fsm01.pem" ec2-user@ec2-18-209-11-152.compute-1.amazonaws.com`
- ![](https://i.imgur.com/Gjclzgc.png)
 

