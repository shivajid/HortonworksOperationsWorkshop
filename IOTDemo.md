##Objective

The Objective of this lab is to have the trucking demo up and running.

### Prerequsites:

<pre>
1) The setup scripts for the demo must be run from the Ambari machine. For the lab please run it on sandbox.hortonworks.com.

2) Demo will be installed and run under the root user.

3) wget must be available.
	Check if you have wget running, else execute:
		$ yum -y install wget

6) Start HBase and Storm, if not running.

7) Turn off maintenace mode for HBase, Storm, Kafka, Falcon and Spark.

9) If running on sandbox:
	- ensure there is at least 8GB of RAM assigned to VM
	- ensure the hosts file is correct: in /etc/hosts, ensure hostname (e.g. sandbox.hortonworks.com) is mapped to actual IP of VM instead of 127.0.0.1
</pre>	

- set JAVA_HOME, if not set.

<pre>

</pre>

## Instructions to install demo:
 -  Install git:
 	<pre>
 		yum -y install git
 	</pre>
-  git clone https://github.com/shivajid/iot-truck-streaming.git

If you get error building this - Clone the below repo

- https://github.com/DhruvKumar/hadoop-mini-clusters
- Build this
- 	mvn clean install -DskipTests

- copy the demo's directory (iot-truck-streaming/) to the local filesystem under /root

- make the scripts executable:
- 
<pre>  
  cd iot-truck-streaming/
  chmod 750 *.sh setup/bin/*.sh
</pre>

- update config.properties with host names where your services run,  including the names of the supervisor nodes
	- NOTE: the demo will pick up the version of config.properties in etc/storm_demo at runtime

- update variables defined at the top in user-env.sh
	-user is ambari user (admin)
	-pass is ambari password (admin)
	-cluster is the name of cluster you will install demo on
	-host is the ambari url, eg: hdpdemo.hortonworks.comt:8080

- Execute the script installdemo.sh
		./installdemo.sh
		
	(If the script is stuck trying to Stop Falcon. You would need to stop Falcon from Ambari.)
	
- source root's bashrc ". /root/.bashrc"

- If on sandbox, run 'rundemo.sh clean', else run 'rundemo.sh'

- When you see the 
	"
	[INFO] Started Jetty Server" message, the demo is up at: 
	http://sandbox.hortonworks.com:8081/storm-demo-web-app/index.html
	
	"


In a subsequent run, you may want to do 'rundemo.sh clean' 
	- which will kill the topology, stop storm, cleanup storm dirs, and restart storm.

