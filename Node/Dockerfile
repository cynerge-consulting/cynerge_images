# Based on Node image, but with a bunch of goodies installed. Notably: Docker, Kubectl, Helm and AWS IAM Authenticator
FROM node

RUN mkdir -p /cynerge
WORKDIR /cynerge

RUN apt update -y && apt upgrade -y
RUN apt install wget
RUN apt install curl
RUN apt install -y apt-transport-https ca-certificates curl software-properties-common apt-transport-https
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
RUN add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
RUN apt update -y
RUN apt-cache policy docker-ce
RUN apt install -y docker-ce
RUN apt-get install -y libxss1 libappindicator1 libindicator7
RUN wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
RUN apt install -y ./google-chrome*.deb
RUN curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
RUN echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | tee -a /etc/apt/sources.list.d/kubernetes.list
RUN apt-get update
RUN apt-get install -y kubectl
RUN curl -o aws-iam-authenticator https://amazon-eks.s3-us-west-2.amazonaws.com/1.12.7/2019-03-27/bin/linux/amd64/aws-iam-authenticator
RUN chmod +x ./aws-iam-authenticator
RUN mkdir -p $HOME/bin && cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator && export PATH=$HOME/bin:$PATH && cp ./aws-iam-authenticator /usr/bin/aws-iam-authenticator
RUN echo export PATH=$HOME/bin:$PATH >> ~/.bashrc