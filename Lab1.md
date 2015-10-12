## HDP2.3 Installation on Single Node CentOS

In this step we will start creating the HDP Compute Cluster. We will create VM with CentOS 6.7. Deploy HDP 2.3 with Ambari 2.1 on a single node. 

There are 2 modes of doing this lab
a) On local CentOS VM (Shared in the page)
b) On Azure trial Account. Follow the Azure Market place to create a Azure VM.
c) CentOS Docker Image. Use the DockerFile in the github.

All three of the above versions will create a CentOS image to run your labs on. The refernce document for Installing HDP is in the official Hortonworks documentation

http://docs.hortonworks.com/HDPDocuments/Ambari-2.1.2.0/bk_Installing_HDP_AMB/content/index.html

Following Lab Instructions are on CentOS VM. These instructions are for reference only.

### OS setup

#### Setup the CentOS VM.

Assign CPU - 4cores
Assign RAM - 10GB
Assign Disk - 40 GB

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
	
	
change hostname from localhost.localdomain to hdpdemo.hortonworks.com

[root@localhost ~]# cat /etc/sysconfig/network
NETWORKING=yes
HOSTNAME=hdpdemo.hortonworks.com
</pre>

+ Download the following scripts. This helps in automated deployment of Ambari and its agents. This is a Hortonworks field generated scripts. These are for lab puprose only. They do the OS pre-checks and cleanup and Installs Ambari Server and the Ambari Agent. You could alternatively follow the official documentation.

<pre>
	wget https://www.dropbox.com/s/s91lintb4xhrqic/ambariInstall.sh?dl=0 -O ambariInstall.sh
	$ chmod +x ambariInstall.sh
</pre>

Run the scripts
<pre>
	$ ./ambariInstall.sh
</pre>

###### [Note:- The above scripts installs Ambari Server and the agent and starts up the ambari server. If you do not want to run the above steps you can do it manually by following the documentation from http://docs.hortonworks.com.]

The next steps is use the Ambari to install HDP.

+ Browse to http://__REPLACE__$ambarihost:8080/.
+ Login using the following account:
<pre>
Username: admin
Password: admin
</pre>
 
#### Deploy a Hortonworks Hadoop Cluster with Isilon for HDFS


+ Login to Ambari Server.
++ admin/admin

![Ambari login](/images/Screen%20Shot%202015-10-11%20at%2012.47.16%20PM.png)

+ Welcome: Specify the name of your cluster. Lets call it <b>hdpdemo</b>
![Create your Cluster](/images/Screen Shot 2015-10-11 at 12.47.33 PM.png)

+ Select the name of the cluster as HDP Demo

!(images/Screen Shot 2015-10-11 at 12.47.50 PM.png)
Click Next

+ Select Stack: Select the HDP 2.3 stack.
![Screen Shot](https://github.com/shivajid/hwx-isilon/blob/master/images/screen1.png)

### Install Options:

+ Specify your Linux hosts that will run HDP for your HDP cluster installation in the Target Hosts text box.
![](/images/Screen%20Shot%202015-10-11%20at%2012.48.24%20PM.png)
+ Choose Perform Manual install

+ Click Next. You may see a warnings. Ignore them for the lab only.

### Choose Services: 

Select all the services

### Assign Masters:

+ Assign all components to the master nodes to HDP components

### Assign Slaves and Clients: 

Rest of the components are assigned to the Compute node (HDP Sandbox)

### Customize the services

+ All the services with redmark needs attention. Supply a password of your choice.



+ Enter the password for all the required services

+ Review: Carefully review your configuration and then click Deploy.
 
+ After a successful installation, Ambari will start and test all of the selected services.  
+ 
+ Sometime it may fail for the first time around. This is mostly due to timeouts. You may need to retry couple of times. If the services do not start, go to the Service tab and restart each of the services.

+ Review the Install, Start and Test page for any warnings or errors. It is recommended to correct any warnings or errors before continuing.

### Validation

Login to Ambari using admin/admin
Under MapReduce, run the “Service Check”


# Thank You!

Issues - Email sdutta@hortonworks.com
