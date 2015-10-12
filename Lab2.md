# docker

In this Lab we are going to use a Docker Container to create second node to the one already installed.

<pre>
a) We are going to Install Docker in the CentOS VM
b) Create Centos Docker Image using a DockerFile
c) Instantiate a container
c) Use the AmbariUi to add a Node
</pre>

## Install Docker Binaries

### CentOS 7
The following steps are to setup docker on CentOS 7.
<pre>
<code>
   $ sudo yum update

   $ curl -sSL https://get.docker.com/ | sh

   $ sudo service docker start

   $ sudo docker run hello-world
</code>
</pre>

To create the docker group and add your user (not needed if you are root user):

* Log into Centos as a user with sudo privileges.
* Create the docker group and add your user.
* <code> sudo usermod -aG docker your_username </code>
* Log out and log back in.
* This ensures your user is running with the correct permissions.
* Verify your work by running docker without sudo.

<pre>
 <code> $ docker run hello-world</code>
</pre>
Ensure Docker is running as a service
<pre><code>
  sudo chkconfig docker on
</pre></code>

### Boot2Docker for Mac
  https://docs.docker.com/mac/step_one/

## Create Docker images 

Use the Dockerfile in the repo to create a CentOS image for HDP Slave Node
<pre>
<code>
  $ mkdir dockerimage
  $ cp DockerFile dockerimage/
  $ docker build --file=dockerimage/DockerFile ./dockerimage
</code>
</pre>

+ This will list the newly create image
<pre>
<code>
  $ docker images 
</code>
</pre>

+ Tag the image
<pre>
<code>
  $docker tag shivajid/centos:hdp23 __imageid__
</code>
</pre>

+ Create the docker instance. 

<pre>
<code>
  $docker run -it -h node0 -P --privileged=true shivajid/centos:hdp23 /bin/bash
</code>
</pre>

+  User CTRL (P+Q) to exit the container. <b>Do not use exit</b> command.

+ List all the running containers
<pre><code>
  $docker ps
</code> </pre>

#  Install Slave Node on the docker images

+ Use the Ambari wizard to add the docker container. 

+ Under the Hosts -> Actions
![Add Hosts](/images/Screen%20Shot%202015-10-11%20at%2010.48.10%20PM.png)

+ Choose the name of the host. In our lab case it is <b><code>node0</code></b>Select you private ssh key [log into the docker container, cat the id_rsa key and provide the information under Provide your ssh key. 
![Put the node information](/images/Screen%20Shot%202015-10-11%20at%2010.52.10%20PM.png) 

+ Ignore the host checks for the lab. (Please take care in your actual lab sessions)

![Warning](/images/Screen Shot 2015-10-11 at 10.54.42 PM.png) 

+ Select the service (Select atleast the DataNode and NodeManager)
 ![](/images/Screen Shot 2015-10-11 at 10.55.04 PM.png)

+ Click Next and start the install process
![Install](/images/Screen Shot 2015-10-11 at 10.55.48 PM.png)
![Install](/images/Screen%20Shot%202015-10-11%20at%2010.58.00%20PM.png)
![install](/images/Screen Shot 2015-10-11 at 10.58.07 PM.png)

## Issues and Resolution

#### Setup the ssh keys
<pre>
<code>
$service sshd star
$ssh-copy-id root@node0
$ ifconfig

</code>
</pre>

+  Add an entry for node0 to the host /etc/host file
+ Update /etc/hosts/ file for <b>node0</b> Docker Container  for ip address mapping for hdpdemo.hortonworks.com

Services on the node may fail to start. It looks for /usr/java/default which is missing. Create an appropriate link to /usr/java/default 

# Docker Reference Commands
+  Stop a container

  $docker stop __container_name_OR_id

+  Remove a container

  $docker rm __container_name_OR_id

+ To Validate. The container should be gone

  $ docker ps

+  To delete an image. Get the image id from running "docker image"

  $docker rmi __image_id

+  To Validae. Run 

  $ docker image


