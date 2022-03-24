# Install spark step by step on your MAC machine

- [Install spark step by step on your MAC machine](#install-spark-step-by-step-on-your-mac-machine)
  - [Step 1: Install JAVA on your MAC](#step-1-install-java-on-your-mac)
  - [Step 2: Install Spark on your MAC](#step-2-install-spark-on-your-mac)
  - [Step 3: Installing `python3`](#step-3-installing-python3)
  - [Step 4: Running Pyspark shell](#step-4-running-pyspark-shell)
  - [Running small program with PySpark Shell](#running-small-program-with-pyspark-shell)
  - [Analyzing Spark Jobs using Spark Context Web UI](#analyzing-spark-jobs-using-spark-context-web-ui)

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

## Step 4: Running Pyspark shell

Now run `pyspark` to see the spark shell in python.

![](https://i.imgur.com/kSPwkyN.png)

---

## Running small program with PySpark Shell 

You will learn about spark shell, local cluster, driver, executor and Spark Context UI. 

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

