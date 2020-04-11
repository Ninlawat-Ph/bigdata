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

1.Set Public Network ip:

