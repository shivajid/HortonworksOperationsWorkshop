## HDP CentOS Node
In this step we will start creating the HDP Compute Cluster. We will create VM with CentOS 7. Deploy HDP 2.3 and connect to Isilon. We will create VM from CentOS iso.

### OS setup

#### Setup the CentOS VM.

Assign CPU - 4cores
Assign RAM - 10GB

+ Download the CentOS 6.7 Iso
https://www.dropbox.com/s/njx0zwmic7og6db/CentOS-6.7-x86_64-bin-DVD1.iso?dl=0

Use the iso to create an new VM. Assign an admin user and password. You should be able to use the same password for your <b>root</b> user.

Next Steps use the following Scripts to create single node VM. Once you have the centos up and running, get the IP address of the machine.

### Installation Steps

+ ssh into the machine as root. Get the ipaddress of the machine

<pre>
	$ ifconfig
</pre>

Assign a hostname to your machine (I will use hdpdemo.hortonworks.com)
<pre>
	$ hostname hdpdemo.hortonworks.com
	$ vi /etc/hosts
		Map the ipaddress in step a to the hostname
	$ vi /etc/sysconfig/network
	
	vi /etc/sysconfig/network
	
change hostname from localhost.localdomain to hdpdemo.hortonworks.com

[root@localhost ~]# cat /etc/sysconfig/network
NETWORKING=yes
HOSTNAME=hdpdemo.hortonworks.com
</pre>

+ Download the following scripts. This helps in automated deployment of Ambari and its agents.
<pre>
	wget https://www.dropbox.com/s/s91lintb4xhrqic/ambariInstall.sh?dl=0 -O ambariInstall.sh
	$ chmod +x ambariInstall.sh
</pre>

Run the scripts
<pre>
	$ ./ambariInstall.sh
</pre>

###### [Note:- The above scripts installs Ambari Server and the agent and starts up the ambari server. If you do not want to run the above steps you can do it manually by following the documentation from http://docs.hortonworks.com.]

The next steps is use the Ambari Install to install HDP.

+ Browse to http://__REPLACE__$ambarihost:8080/.
+ Login using the following account:
<pre>
Username: admin
Password: admin
</pre>
 
#### Deploy a Hortonworks Hadoop Cluster with Isilon for HDFS


+ Login to Ambari Server.
+ Welcome: Specify the name of your cluster. Lets call it <b>hdpdemo</b>

Click Next

+ Select Stack: Select the HDP 2.3 stack.
![Screen Shot](https://github.com/shivajid/hwx-isilon/blob/master/images/screen1.png)

### Install Options:

Specify your Linux hosts that will run HDP for your HDP cluster installation in the Target Hosts text box and the Isilon Zone Name Node IP Address.
Choose Perform Manual install
Click Next. You may see a warnings. Ignore them.

### Choose Services: 
Select all the services. There are more services in HDP 2.3 than in this screenshot

### Assign Masters:

+ Assign NameNode and SNameNode components to the Isilon. 
+ Assign the rest of the nodes to HDP components

### Assign Slaves and Clients: 

Assign the DataNode to Isilon host (No Client)
Rest of the components are assigned to the Compute node (HDP Sandbox)

### Customize the services

+ Change the port for webhdfs to 8082 under HDFS. The default address is 50070.

<pre>
dfs.namenode.http-address = __REPLACE__hostname__:8082
</pre>

+ Enter the password for all the required services

+ Review: Carefully review your configuration and then click Deploy.
 
+ After a successful installation, Ambari will start and test all of the selected services.  Sometime it may fail for the first time around. You may need to retry couple of times. Review the Install, Start and Test page for any warnings or errors. It is recommended to correct any warnings or errors before continuing.

### Validation

Login to Ambari using admin/admin
Under MapReduce, run the “Service Check”


# Thank You!

Issues - Email sdutta@hortonworks.com
