# Get-started-with-Hadoop-Spark

I. Launch and close your Virtual Machine
	Launch the VM
	Close the VM
II. Try pySpark

I. Launch and close your Virtual Machine
    Launch the VM

Download VirtualBox
Download the VM and add it
Launch the VM and complete both login and password with `hadoop`.
Now run the following commands in its terminal:
`start-dfs.sh` to launch the data file system
‘start-yarn.sh` to launch the yarn ressources manager
`jps` to handle Java processes

Now you’ll need VM’s ip, run `ip addr show`.
Copy it so you can access the web interface at this url: ip:50070 (HDFS) or ip:8088 (Ressource Manager)

Close the VM

If you want to close your VM, run:
`stop-yarn.sh`
`stop-dfs.sh`
`sudo poweroff`



II. Try pySpark

If you want to work on Jupyter notebooks, run `cd notebooks`, `jupyter notebook`.
Get back to your browser and type the following url: ip:9090. Again, the password is `hadoop`.

```
from pyspark import SparkContext, HiveContext, SparkConf
import matplotlib.pyplot as plt
```

```
conf = SparkConf().setMaster('local[*]')
conf = conf.setAppName('APPTEST')
conf = conf.set('spark.ui.port', '4042')
sc = SparkContext (conf=conf)
hctx = HiveContext(sc)
hctx.sql('SHOW DATABASES').show()
```
