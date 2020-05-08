# How To Install Hadoop 3 On Linux


## Step 1 : Installing Java
	* Install Java First
	* java -version

## Step 2 : Installing Hadoop
	$sudo wget https://downloads.apache.org/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz
	$sudo tar zxvf hadoop-* -C /usr/local
	$sudo mv /usr/local/hadoop-* /usr/local/hadoop
	

Step 3 :  Configure environment
	6.sudo nano ~/.bashrc
    	**.Add the following path variables in it (verify ur java Path in first line)
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

# Load configure
	7.source ~/.bashrc
		
Modify First FILE : hadoop-env.sh
	8.cd /usr/local/hadoop/etc/hadoop
  	**define Java Path 
    	**verify JAVA path in your system with command :  readlink -f /usr/bin/java | sed "s:bin/java::"
	9.sudo nano hadoop-env.sh
    	**Add line :(Java path )
	    export JAVA_HOME=/usr/lib/jvm/java-14-oracle/

Modify Second FILE : core-site.xml 
	10. sudo nano core-site.xml
    	**Add Lines between <onfiguration> </configuration>
	
	<property>
	    <name>hadoop.tmp.dir</name>
	    <value>/app/hadoop/tmp</value>
	  </property>
	<property>
	    <name>fs.defaultFS</name>
	    <value>hdfs://localhost:54310</value>
	  </property>

Modifie Third FILE : hdfs-site.xml
	**befor do that create two folder ,one for namenode and other for datanode
	12. mkdir -p /usr/local/hadoop_store/hdfs/namenode
	13. mkdir -p /usr/local/hadoop_store/hdfs/datanode
        14.sudo nano hdfs-site.xml
	#Add lines :
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

MOdify THE FOURTH FILE : mapred-site.xml
	15 .sudo nano  mapred-site.xml
	**.Add Lines:
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

Modify The fifth FILE: yarn-site.xml
	16. sudo nano yarn-site.xml
 	**Add lines :
	
	<property>
	<name>yarn.nodemanager.aux-services</name>
	<value>mapreduce_shuffle</value>
	</property>

Step 4 :CREATE HAdoop file system
	hdfs namenode -format
Step 5 :Start Hadoop Cluster 
	cd $HADOOP_HOME/sbin
	start-dfs.sh
	jsp 
	** to verify from web navigator 
	# NameNode 	   : http://localhost:9870/
	# RessourceManager  : http://localhost:8088/
	# 

##################### Configure Eclipse 
	1.download and install eclipse Luna 
	2.copy jar files to dropins directory inside eclipse directory
