FROM hitesh/serf-dnsmasq:2.0

MAINTAINER Hitesh <hitesh.m01@gmail.com>

# install openssh-server, vim and openjdk
RUN apt-get update
RUN apt-get -y install openssh-server openssh-client

# move all configuration files into container
ADD files/bashrc /usr/local/
ADD files/hadoop-env.sh /usr/local/
ADD files/spark-env.sh /usr/local/
ADD files/hive-env.sh /usr/local/
ADD files/ssh_config /usr/local/
ADD files/hadoop /usr/local/hadoop/
# ADD files/scala-2.10.4 /usr/local/scala-2.10.4
ADD files/spark /usr/local/spark
ADD files/mysql-connector-java-5.1.6.jar /usr/local/mysql-connector-java-5.1.6.jar
ADD files/hive /usr/local/hive
ADD files/hive-hwi.war /usr/local/hive-hwi.war

# set jave environment variable
# ENV JAVA_HOME /usr/lib/jvm/jre-1.7.0-openjdk.x86_64
# ENV PATH $PATH:$JAVA_HOME/bin
RUN java -version

#install scala 2.10.4
# RUN ln -s /usr/local/scala-2.10.4 /usr/local/scala
# ENV SCALA_HOME=/usr/local/scala
# ENV PATH $PATH:$SCALA_HOME/bin
#RUN scala -version

#configure ssh free key access
RUN mkdir /var/run/sshd && \
ssh-keygen -t rsa -f ~/.ssh/id_rsa -P '' && \
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys && \
mv /usr/local/ssh_config ~/.ssh/config && \
chmod 600 ~/.ssh/authorized_keys && \
sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

#install hadoop 
RUN ln -s /usr/local/hadoop /usr/local/hadoop && \
ls /usr/local/hadoop && \
mv /usr/local/hadoop-env.sh /usr/local/hadoop/hadoop-env.sh

#install spark 
RUN ln -s /usr/local/spark /usr/local/spark
ENV SPARK_HOME /usr/local/spark
ENV PATH $PATH:$SPARK_HOME/bin
RUN ls /usr/local/spark && \
mv /usr/local/spark-env.sh /usr/local/spark/conf/spark-env.sh

# install hive 1.2.1
RUN ln -s /usr/local/hive /usr/local/hive
ENV HIVE_HOME /usr/local/hive
ENV PATH $PATH:$HIVE_HOME/bin
RUN mv /usr/local/hive-env.sh /usr/local/hive/conf/hive-env.sh
# RUN mv /usr/local/hive-hwi.war /user/local/hive/lib/hive-hwi.war
# install mysql-java-connecter for hive
RUN ln -s /usr/local/mysql-connector-java-5.1.6.jar /usr/local/hive/lib/mysql-connector-java-5.1.6.jar


RUN mv /usr/local/bashrc ~/.bashrc
