# Get-started-with-pySpark-and-Hive

I. Launch and close a Virtual Machine
	Launch the VM
	Close the VM
II. Build Hive database
III. Try pySpark

## I. Launch and close your Virtual Machine
### Launch the VM

Download VirtualBox.  
Download the VM and add it.  
Launch the VM and complete both login and password with `hadoop`.  
Now run the following commands in its terminal:  
- `start-dfs.sh` to launch the data file system.  
- `start-yarn.sh` to launch the yarn ressources manager.  
- `jps` to handle Java processes.  

Now you’ll need VM’s _ip_, run `ip addr show`.  
Copy it so you can access the web interface at this url: _ip:50070_ (HDFS) or _ip:8088_ (Ressource Manager).  

### Close the VM

If you want to close your VM, run:  
- `stop-yarn.sh`  
- `stop-dfs.sh`  
- `sudo poweroff`  


## II. Build a Hive database

- `hdfs dfs -ls /`
- `cd public`
- `ls`
- `hdfs dfs -mkdir /user/hadoop/data`
- `hdfs dfs -put dat_svi_data.csv /user/hadoop/data/`

- `hive`
- `CREATE DATABASES;`


## III. Try pySpark

If you want to work on Jupyter notebooks, run `cd notebooks`, then `jupyter notebook`.  
Get back to your browser and type the following url: _ip:9090_. Again, the password is `hadoop`. #this will give you a web interface for Spark at ip:4042.  

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
%matplotlib inline
plt.ylabel('calis per halfhour')
plt.title('Total Calis')
plt.plot(hdata.values().collect())
```
