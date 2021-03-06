#
# Copyright 2014-2015 Red Hat, Inc. and/or its affiliates
# and other contributors as indicated by the @author tags.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

# Heapster Docker Base Image
FROM centos:centos7

# The image is maintained by the Hawkular Metrics team
MAINTAINER Hawkular Metrics <hawkular-dev@lists.jboss.org>

EXPOSE 8082

#Commit for version unreleased v1.1.0-beta1 (SNAPSHOT)
ENV HEAPSTER_COMMIT=de510e4


RUN yum install -y -q go git wget make && \
    yum clean all && \
    cd /tmp && \
    ## TODO: remove this section once centos7 has access to go 1.5+
    wget https://storage.googleapis.com/golang/go1.5.3.linux-amd64.tar.gz && \
    tar xvf go1.5.3.linux-amd64.tar.gz && \
    export GOROOT=/tmp/go && \
    export PATH=$GOROOT/bin:$PATH && \
    ## END TODO
    go version && \
    export GOPATH=/tmp/gopath && \
    export PATH=$PATH:$GOPATH/bin && \
    mkdir -p $GOPATH/src/k8s.io && \
    cd $GOPATH/src/k8s.io && \
    git clone https://github.com/DafuSHI/heapster.git && \
    cd heapster && \
    git checkout hw-ES-sink && \
    make && \
    cp heapster /opt && \
    rm -rf $GOPATH && \
    yum remove -y -q go git wget make
    
ENV PATH=$PATH:/opt

ENTRYPOINT ["/opt/heapster"]

#Generate dockerhub.gemalto.com:8500/dafushi/heapster_base:latest 
