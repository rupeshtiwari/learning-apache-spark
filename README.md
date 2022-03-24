# Install & Run Spark on your MAC machine & AWS Cloud step by step guide

http://www.rupeshtiwari.com/learning-apache-spark/

- [Install & Run Spark on your MAC machine & AWS Cloud step by step guide](#install--run-spark-on-your-mac-machine--aws-cloud-step-by-step-guide)
  - [Step 1: Install JAVA on your MAC](#step-1-install-java-on-your-mac)
  - [Step 2: Install Spark on your MAC](#step-2-install-spark-on-your-mac)
  - [Step 3: Installing `python3`](#step-3-installing-python3)
  - [Step 4: Running PySpark shell in your MAC laptop](#step-4-running-pyspark-shell-in-your-mac-laptop)
- [Running small program with PySpark Shell in your MAC laptop](#running-small-program-with-pyspark-shell-in-your-mac-laptop)
  - [Analyzing Spark Jobs using Spark Context Web UI in your MAC laptop](#analyzing-spark-jobs-using-spark-context-web-ui-in-your-mac-laptop)
- [Running Jupyter Notebook ( In local cluster and client mode ) in your MAC laptop](#running-jupyter-notebook--in-local-cluster-and-client-mode--in-your-mac-laptop)
  - [Step 1: Setting environment variable and starting notebook](#step-1-setting-environment-variable-and-starting-notebook)
  - [Step 2: installing `findspark`](#step-2-installing-findspark)
  - [Step 3 connecting spark with notebook shell](#step-3-connecting-spark-with-notebook-shell)
- [Installing Multi-Node Spark Cluster in AWS Cloud](#installing-multi-node-spark-cluster-in-aws-cloud)
  - [Step 1: Creating EMR cluster at AWS cloud](#step-1-creating-emr-cluster-at-aws-cloud)
      - [What is Amazon EMR?](#what-is-amazon-emr)
  - [Step 2: Running PySpark on EMR cluster in AWS cloud using Spark-Shell](#step-2-running-pyspark-on-emr-cluster-in-aws-cloud-using-spark-shell)
  - [Step 3: Running PySpark on Notebook on EMR cluster at AWS cloud using Zeppelin](#step-3-running-pyspark-on-notebook-on-emr-cluster-at-aws-cloud-using-zeppelin)
  - [Working with `spark-submit` on EMR cluster](#working-with-spark-submit-on-emr-cluster)

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

## Step 4: Running PySpark shell in your MAC laptop

Now run `pyspark` to see the spark shell in python.

![](https://i.imgur.com/kSPwkyN.png)

# Running small program with PySpark Shell in your MAC laptop

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

## Analyzing Spark Jobs using Spark Context Web UI in your MAC laptop

To monitor and investigate your spark application you can check spark context web UI.

- Go to url http://localhost:4040/jobs/
- Check Event Timeline Spark started and executed driver process
  ![](https://i.imgur.com/MJzJgmU.png)
- We are not seeing separate executer process, because we are in local cluster. Every thing is running in single JVM. JVM is a combination of driver and executer.
  ![](https://i.imgur.com/LdzomXg.png)
- When cluster created we did not pass number of thread so it took default number as `8` based on my laptop hardware resource available.
- Storage Memory it took maximum `434.4 MB`. This is sum of overall JVM.
- You can access this spark context UI till your spark shell is open. Once you quit spark shell you will loose the access to this UI.

# Running Jupyter Notebook ( In local cluster and client mode ) in your MAC laptop

Data scientist use `Jupyter` Notebook to develop & explore application step by step. Spark programming in python requires you to have python on your machine.

| cluster | mode        | tool     |
| ------- | ----------- | -------- |
| Local   | Client Mode | Notebook |

If you install Anaconda environment, then you get python development environment also you will get spark support. You can [download](https://www.anaconda.com/products/individual) the community edition and install Anaconda. ANaconda comes with pre-configured `Jupyter` notebook.

How to use Spark using `Jupyter` Notebook?

Notebook is a Shell based environment. You can type your code in shell and run it.

1. set `SPARK_HOME` environment variable
2. Install `findspark` package
3. Initialize `findspark`: the connection between Anaconda python environment and your spark installation

## Step 1: Setting environment variable and starting notebook

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

## Step 2: installing `findspark`

```s
# 1. install pipx
brew install pipx

#2. install findspark
pip3 install findspark
```

![](https://i.imgur.com/er4r4dz.png)

## Step 3 connecting spark with notebook shell

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

# Installing Multi-Node Spark Cluster in AWS Cloud

At AWS, Amazon EMR (Elastic Map & Reduce) service can be used to create `Hadoop` cluster with `spark`.

| cluster | mode        | tool                  |
| ------- | ----------- | --------------------- |
| YARN    | Client Mode | spark-shell, Notebook |

This mode is used by data scientist for interactive exploration directly with production cluster. Most cases we use notebooks for web base interface and graph capability.

## Step 1: Creating EMR cluster at AWS cloud

Creating spark shell on a real multi-node yarn cluster.

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
- Note you get `3 slave (executer) and 1 master (driver)` EC2 instances created.
- Go to security group of master node, add new rule, and allow all traffic from your IP address.
- SSH to Master instance. `ssh -i "fsm01.pem" hadoop@ec2-18-209-11-152.compute-1.amazonaws.com`

  ![](https://i.imgur.com/mmYNBPJ.png)

👉 make sure to login with `hadoop` user

## Step 2: Running PySpark on EMR cluster in AWS cloud using Spark-Shell

- Run `pyspark` to create spark shell, when you want to quit the shell then press control D.
  ![](https://i.imgur.com/Hs2B3Ek.png)
- My spark shell is running. My driver and executers already created and waiting for me to submit spark command.
  ![](https://i.imgur.com/Y4JTzTa.png)
- You can see spark context UI to analyze the job by clicking on spark history server in EMR cluster at AWS.
  ![](https://i.imgur.com/FniKv4B.png)
- Go to the spark history server URL
  - It will show you list of application that you executed in the past.
  - Currently it is not showing any application so go ahead and close your pyspark shell and you see as many times you have opened pyspark and closed it they all are treated as a application.
    ![](https://i.imgur.com/Ohi0lwn.png)
  - I closed pyspark shell 4 times.
  - Open any one and go to time line events.
    ![](https://i.imgur.com/2lTaraS.png)
  - Note you got 1 driver and 3 executers. That is what you asked when you created your cluster.
  - Click on executers tab and note you get 3 executers and check their memory allocation.
    ![](https://i.imgur.com/0pb69s4.png)

## Step 3: Running PySpark on Notebook on EMR cluster at AWS cloud using Zeppelin

👉 Note: mostly you will not use pyspark shell in real world people are using notebooks.
Therefore, we are going to use zeppelins notebook next.

**Visit Zeppelin URL**

![](https://i.imgur.com/HAu2Nmt.png)

In your secured enterprise setup you have to ask your cluster operations team to provide you the URL and grant you the access for the same.

Notebook is not like spark shell. So It is not connected to spark by default you have to run some spark command to connect. You can simply run `spark.version` command also.

![](https://i.imgur.com/wfSK2PV.png)

Create new notebook and run `spark.version` Default notebook zeppelin shell is skala shell. Therefore, you should use interpreter directive `%pyspark` so that you can run python code.

![](https://i.imgur.com/9PhPHwP.png)

## Working with `spark-submit` on EMR cluster

| cluster | mode         | tool         |
| ------- | ------------ | ------------ |
| YARN    | Cluster Mode | spark-submit |

This mode of operation is mostly used for executing spark application on your production cluster. `spark-submit --help` to check all options. 

Let's create and submit a spark application. 

- create `main.py` in master node

```py
import sys
x=int(sys.argv[1])
y=int(sys.argv[2])
sum=x+y
print("The addition is :",sum)
```

- Submit application `spark-submit main.py 1 3`

![](https://i.imgur.com/2cfBOvx.png)



https://learning.oreilly.com/videos/strata-hadoop/9781491924143/9781491924143-video210705/ 