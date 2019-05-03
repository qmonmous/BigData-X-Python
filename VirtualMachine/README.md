# Get-started-with-pySpark-and-Hive

*This repository aims at providing an introduction to Big Data tools through Hadoop, Hive and Spark.*

---

When it comes to large amount of data you need specific tools to scale and efficiently handle the processes.
We'll discover three of the most popular tools that can easily work together Hadoop, Hive and focus on Spark.

I. From Hadoop to Spark  
II. Hive and Spark SQL  
III. Deep dive in pySpark

## I. From Hadoop to Spark

MapReduce is a programming pattern used by Apache Hadoop. Hadoop MapReduce works in providing the systems that can store, process, and mine huge data with parallel multi node clusters in a scalable, reliable, and error-absorbing inexpensive distributed system. In MapReduce, the data analysis and data processing are split into individual phases called the Map phase and Reduce.

Let's find out how it works with the word count example. 

![Test Image](img/mapreduce.png)

Par exemple, s’il est possible de compter manuellement le nombre de fois qu’un mot apparaît dans un roman, cela prend beaucoup de temps. Si l’on répartit cette tâche entre une vingtaine de personnes, les choses peuvent aller beaucoup plus vite. Chaque personne prend une page du roman et écrit le nombre de fois que le mot apparaît sur la page. Il s’agit de la partie Map de MapReduce. Si une personne s’en va, une autre prend sa place. Cet exemple illustre la tolérance aux erreurs de MapReduce. Lorsque toutes les pages sont traitées, les utilisateurs répartissent tous les mots dans 26 boîtes en fonction de la première lettre de chaque mot. Chaque utilisateur prend une boîte, et classe les mots par ordre alphabétique. Le nombre de pages avec le même mot est un exemple de la partie Reduce de MapReduce.

The main issue with MapReduce is that at each step, the file system (called HDFS) read and write and loose a lot of performance by saving each intermediary outputs. Building a better algorithm would require a primitive to share the data efficiently. Here comes Spark with his ability to store iterations in your cache and computer memory to avoid moving data. By distributing the work in partitions between different nodes thanks to his Resiliend Distributed Dataset (RDD), Spark is up to 30 times faster than Hadoop MapReduce for things like sorting.
Moreover, Spark adds some great features like replication of its partitions on different nodes.

Spark works in four steps:
- create a RDD using an external source or a collection,
- aplly several transformations to define new RDDs (as RDD is immutable and functions like `map` or `filter` will lead to a transformed RDD creation),
- Ask Spark to keep in memory the different RDDs you'll need thanks to `persist` and `unpersist`,
- Run several actions optimised by Spark like `.count()` or `.collect` that are immediatly stored in memory.

<<<< IMAGE DES DIFFERENTES TRASNFORMATIONS ET DIFFERENT ACTIONS AVAILABLE ONO SPARK >>>>


We are now going to build a Big Data environment with HDFS to focus on Spark queries and how it works with a database like Hive.

### Simulate an Hadoop environment in a VM

Download VirtualBox.  
Download the VM and add it.  
Launch the VM and complete both login and password with `hadoop`.  
Now run the following commands in its terminal:  
- `start-dfs.sh` to launch the data file system.  
- `start-yarn.sh` to launch the yarn ressources manager.  
- `jps` to handle Java processes.  

Now you’ll need VM’s _ip_, run `ip addr show` or `ifconfig`.  
Copy it so you can access the web interface at this url: _ip:50070_ (HDFS) or _ip:8088_ (Yarn Ressource Manager).  

> Tips: You can interact with the VM through your own terminal by running `ssh hadoop@ip` in the terminal. 

#### Close the VM

If you want to close your VM, run:  
- `stop-yarn.sh`  
- `stop-dfs.sh`  
- `sudo poweroff`  


## II. Build a Hive database


Apache Hive can be considered as a neo SQL, designed for Big Data. /// ... blablabla ... ///

- come back to the root: `hdfs dfs -ls /`, then run: `cd public`
- create a repository for your data: `hdfs dfs -mkdir /user/hadoop/data`
- move a file from the current repository to another: `hdfs dfs -put dat_svi_data.csv /user/hadoop/data/`
- launch Hive: `hive`
- create a database: `CREATE DATABASES;`

```sql
Create data structure scheme:
CREATE TABLE svi_data (
    calldate string,
    calltime string,
    callid bigint,
    calltype string,
    calltype2 string,
    phonenumber string,
    userid bigint)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '\;';

LOAD DATA INPATH '/user/hadoop/data/dat_svi_data.csv' INTO TABLE svi_data;
```

Now you can try different SQL requests:
```sql
SELECT * FROM svi_data LIMIT 10;
```
```sql
SELECT COUNT(*) FROM svi_data;
```


## III. Try pySpark

Let's move on Spark. We'll try this tool by practicing in a jupyter notebook. Run `cd notebooks` to move to the notebooks directory, then `jupyter notebook` to build the environment.  
Now open a browser and go to the following url: *ip:9090*. Again, the password is `hadoop`. This will give you a web interface for Spark at ip:4042.  

### First code in Spark

We are going to see how you would do to count words in a textfile with Spark.
```python
from pyspark import SparkContext

sc = pyspark.SparkContext()
file = sc.textfile("data/count.txt")

count = file.flatMap(lambda line: line.split(" ")) #split words on each line
            .map(lambda word: (word, 1)) #add 1 for each occurence of a word
            .reduceByKey(lambda a, b: a + b) #aggregate the number of occurences of each word
count.persist()
count.saveAsTextFile("data/count.txt")

```

```python
from pyspark import SparkContext, HiveContext, SparkConf
import matplotlib.pyplot as plt
```

```python
conf = SparkConf().setMaster('local[*]')
conf = conf.setAppName('APPTEST')
conf = conf.set('spark.ui.port', '4042')
sc = SparkContext (conf=conf)
hctx = HiveContext(sc)
hctx.sql('SHOW DATABASES').show()
```

```
%matplotlib inline
plt.ylabel('calis per halfhour')
plt.title('Total Calis')
plt.plot(hdata.values().collect())
```

