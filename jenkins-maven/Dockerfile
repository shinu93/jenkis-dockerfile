FROM ubuntu

RUN apt update && apt upgrade -y

RUN useradd -ms /bin/bash jenkins && \
    chown -R jenkins:jenkins /home/jenkins && \
    echo "jenkins:12345" | chpasswd && \
    echo "root:12345" | chpasswd

RUN apt install openssh-server -y

RUN mkdir /home/jenkins/.ssh && \
    chown -R jenkins:jenkins /home/jenkins/.ssh && \
    chmod -R 700 /home/jenkins/.ssh

COPY jenkins-node.pub /home/jenkins/.ssh/authorized_keys

RUN chown -R jenkins:jenkins /home/jenkins/.ssh/authorized_keys && \
    chmod 644 /home/jenkins/.ssh/authorized_keys

### installing java and maven###########################
RUN apt install vim wget -y

ENV JAVA_DOWNLOAD="jdk-17_linux-x64_bin.tar.gz"

RUN wget https://download.oracle.com/java/17/latest/${JAVA_DOWNLOAD}

RUN tar -xvzf ${JAVA_DOWNLOAD} && rm -rf ${JAVA_DOWNLOAD}

RUN mv jdk* java && mv java /opt/java

ENV MAVEN_DOWNLOAD=apache-maven-3.9.4-bin.tar.gz

RUN wget https://dlcdn.apache.org/maven/maven-3/3.9.4/binaries/${MAVEN_DOWNLOAD}

RUN tar -xvzf ${MAVEN_DOWNLOAD} && rm -rf ${MAVEN_DOWNLOAD}

RUN mv apache* /opt/maven

ENV M2_HOME=/opt/maven

ENV JAVA_HOME=/opt/java

ENV PATH=$PATH:$JAVA_HOME:$JAVA_HOME/bin:$M2_HOME:$M2_HOME/bin

#######################################################

RUN mkdir /var/run/sshd

CMD /usr/sbin/sshd -D
