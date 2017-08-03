FROM ubuntu:16.04

RUN mkdir /www
WORKDIR /www
RUN apt-get update

RUN apt-get install -y libzmq-dev npm nodejs-legacy python python-pip
RUN apt-get update && \
	apt-get install -y openjdk-8-jdk && \
	apt-get install -y ant && \
	apt-get clean;
	
# Fix certificate issues, found as of 
# https://bugs.launchpad.net/ubuntu/+source/ca-certificates-java/+bug/983302
RUN apt-get update && \
	apt-get install ca-certificates-java && \
	apt-get clean && \
	update-ca-certificates -f;

# Setup JAVA_HOME, this is useful for docker commandline
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
RUN export JAVA_HOME
COPY ./lib/ /opt/lib
RUN pip install --upgrade pip
#COPY requirements.txt /www
#RUN pip install -r requirements.txt
RUN apt-get install -y libatlas-dev
RUN pip install numpy scipy cython
RUN pip install -U /opt/lib/scikit_learn-0.19.dev0-cp27-cp27mu-linux_x86_64.whl
#RUN pip install scikit-learn

RUN pip install zerorpc
RUN pip install tweepy
RUN pip install nltk
RUN pip install elasticsearch
RUN python -m nltk.downloader stopwords

COPY ./Nebula/package.json /www
RUN npm install

COPY ./Nebula /www
EXPOSE 8081

CMD ["npm", "start"]

