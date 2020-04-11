# Is a way to install spark stand alone

## Install JAVA

yum -y update

java -version

yum install java-1.8.0-openjdk

## Set Java’s Home Environment

update-alternatives --config java

vi .bash_profile
pass 

export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre

sudo setenforce 0   (Disable SELinux)

cd opt

yum install wget

## Download Spark 

wget http://d3kbcqa49mib13.cloudfront.net/spark-1.6.0-bin-hadoop2.6.tgz

tar -xzf spark-1.6.0-bin-hadoop2.6.tgz

ln -s /opt/spark-1.6.0-bin-hadoop2.6  /opt/spark

export SPARK_HOME=/opt/spark

export PATH=$PATH:$SPARK_HOME/bin

echo 'export SPARK_HOME=/opt/spark' >> .bash_profile

echo 'export PATH=$PATH:$SPARK_HOME/bin' >> .bash_profile

cd spark

## Start Spark

./sbin/start-master.sh

## putty tunnels

https://blog.devolutions.net/2017/04/how-to-configure-an-ssh-tunnel-on-putty

After you tunnels and then can access to http://localhost:8080/  

## Install netcat 

yum install nc

## Install Python

python --version

yum install -y python36u

## Change config python to python 3.6 (If version python below 3)

Go to this path ‘usr/bin/python’

You see python -> python2.7 old version need to change to python3.6 
