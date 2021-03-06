# Copyright 2017 Red Hat
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
# ------------------------------------------------------------------------
#
# This is a Dockerfile for the testing/openshift-spark:0.4.3 image.

FROM centos:latest

# Environment variables
ENV JBOSS_IMAGE_NAME="testing/openshift-spark" \
    JBOSS_IMAGE_VERSION="0.4.3" \
    PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/opt/spark/bin" \
    SPARK_HOME="/opt/spark" 

# Labels
LABEL name="$JBOSS_IMAGE_NAME" \
      version="$JBOSS_IMAGE_VERSION" \
      architecture="x86_64" \
      com.redhat.component="testing-openshift-spark-docker" \
      maintainer="Chad Roberts <croberts@redhat.com>" \
      sparkversion="2.2.1"


USER root


# Install required RPMs and ensure that the packages were installed
RUN yum install -y  java-1.8.0-openjdk numpy wget \
    && yum clean all && \
    rpm -q  java-1.8.0-openjdk numpy wget

# Add all artifacts to the /tmp/artifacts
# directory
COPY \
    spark-2.2.1-bin-hadoop2.7.tgz \
    /tmp/artifacts/

# Add scripts used to configure the image
COPY modules /tmp/scripts

# Custom scripts
USER root
RUN [ "bash", "-x", "/tmp/scripts/spark/install" ]

USER root
RUN [ "bash", "-x", "/tmp/scripts/metrics/install" ]

USER root
RUN rm -rf /tmp/scripts
USER root
RUN rm -rf /tmp/artifacts

USER 185

# Specify the working directory
WORKDIR /tmp

ENTRYPOINT ["/entrypoint"]

CMD ["/opt/spark/bin/launch.sh"]
