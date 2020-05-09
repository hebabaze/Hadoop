# How To Install Hadoop 3 On Linux


## Step 1 : Installing Java
	* Install Java First
	* java -version

## Step 2 : Installing Hadoop
	$sudo wget https://downloads.apache.org/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz
	$sudo tar zxvf hadoop-* -C /usr/local
	$sudo mv /usr/local/hadoop-* /usr/local/hadoop
	

## Step 3 :  Configure environment
	$sudo nano ~/.bashrc
> Add the following path variables in it (verify ur java Path in first line)
    	
	#JAVA VARIABLE
	export JAVA_HOME=/usr/lib/jvm/java-14-oracle/
	#HADOOP VARIABLES
	export HADOOP_HOME=/usr/local/hadoop
	export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
	export HADOOP_INSTALL=$HADOOP_HOME
	export HADOOP_MAPRED_HOME=$HADOOP_HOME
	export HADOOP_COMMON_HOME=$HADOOP_HOME
	export HADOOP_HDFS_HOME=$HADOOP_HOME
	export YARN_HOME=$HADOOP_HOME
	export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
	export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
	

> Load configure

	$source ~/.bashrc
		
## Step 4:  Modify First FILE : hadoop-env.sh
	$cd /usr/local/hadoop/etc/hadoop
 > define Java Path 
 > verify JAVA path in your system with command :  *readlink -f /usr/bin/java | sed "s:bin/java::"*

	$sudo nano hadoop-env.sh
  > Add line :(Java path )
  
  	export JAVA_HOME=/usr/lib/jvm/java-14-oracle/

## Step 5 : Modify Second FILE : core-site.xml 
	$sudo nano core-site.xml
> Add Lines between : '''***<'configuration> ... </configuration'> ***'''

	<property>
	    <name>hadoop.tmp.dir</name>
	    <value>/app/hadoop/tmp</value>
	  </property>
	<property>
	    <name>fs.defaultFS</name>
	    <value>hdfs://localhost:54310</value>
	  </property>

## Step 6 : Modifie Third FILE : hdfs-site.xml
> befor do that create two folder ,one for namenode and other for datanode
	 
	 $mkdir -p /usr/local/hadoop_store/hdfs/namenode
	 $mkdir -p /usr/local/hadoop_store/hdfs/datanode
         $sudo nano hdfs-site.xml
> Add lines :

	<property>
	<name>dfs.replication</name>
	<value>1</value>
	</property>
	<property>
	<name>dfs.namenode.name.dir</name>
	<value>file:///usr/local/hadoop_store/hdfs/namenode</value>
	</property>
	<property>
	<name>dfs.datanode.name.dir</name>
	<value>file:///usr/local/hadoop_store/hdfs/datanode</value>
	</property>
	</configuration>

## Step 7 : MOdify THE FOURTH FILE : mapred-site.xml
	$sudo nano  mapred-site.xml
> Add Lines:

	<property>
		 <name>yarn.app.mapreduce.am.env</name>
		 <value>HADOOP_MAPRED_HOME=$HADOOP_HOME</value>
		</property>
	<property>
		 <name>mapreduce.map.env</name>
		 <value>HADOOP_MAPRED_HOME=$HADOOP_HOME</value>
	</property>
	<property>
		 <name>mapreduce.reduce.env</name>
		 <value>HADOOP_MAPRED_HOME=$HADOOP_HOME</value>
	</property>

## Step 8 : Modify The fifth FILE: yarn-site.xml

	$sudo nano yarn-site.xml
 > Add lines :

	<property>
	<name>yarn.nodemanager.aux-services</name>
	<value>mapreduce_shuffle</value>
	</property>

## Step 9 :CREATE HAdoop file system
	$hdfs namenode -format
## Step 10 :Start Hadoop Cluster 
	$cd $HADOOP_HOME/sbin
	$start-dfs.sh
	$jsp 
### to verify from web navigator 
	- [x] NameNode 	   : http://localhost:9870/
	- [x] RessourceManager  : http://localhost:8088/
	

##################### Configure Eclipse 
	1.download and install eclipse Luna 
	2.copy jar files to dropins directory inside eclipse directory
