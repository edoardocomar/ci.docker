# (C) Copyright IBM Corporation 2016, 2017
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

FROM ubuntu:16.04

MAINTAINER Bethany Griggs <bethany.griggs@uk.ibm.com> (@BethGriggs)

ENV IBM_VERSION 8.6.0.0
ENV NODE_VERSION 8.6.0

RUN set -eux; \
    buildDeps='wget ca-certificates unzip openjdk-8-jre'; \
    apt-get update && apt-get install -y --no-install-recommends $buildDeps \
      && rm -rf /var/lib/apt/lists/*; \
    AMD64_SHA="36b5e323fb53549cd4ce42f9a73ca0bfab47a00d9a7dbc9058e69cce9744821a"; \
    PPC64LE_SHA="e74cd9a3c14fe52cab34673938090593161e181b6469dfdc14f4a3f99b8d665"; \
    S390X_SHA="9bc142c7912f86a66e608d36e8615072a62ee898b1e3e163d1b4468a96a17abb"; \
    ARCH="$(dpkg --print-architecture)"; \
    case "${ARCH}" in \
       amd64|x86_64) \
         BINARY="$IBM_VERSION/linux/x86_64/ibm-$IBM_VERSION-node-v$NODE_VERSION-linux-x64.bin"; \
         SHA=$AMD64_SHA; \
         ;; \
         ppc64el|ppc64le) \
         BINARY="$IBM_VERSION/linux/ppc64le/ibm-$IBM_VERSION-node-v$NODE_VERSION-linux-ppc64le.bin"; \
         SHA=$PPC64LE_SHA; \
         ;; \
         s390x) \
         BINARY="$IBM_VERSION/linux/s390x/ibm-$IBM_VERSION-node-v$NODE_VERSION-linux-s390x.bin"; \
         SHA=$S390X_SHA; \
         ;; \
       *) \
         echo "Unsupported arch: ${ARCH}"; \
         exit 1; \
         ;; \
    esac; \
    BASE_URL="https://public.dhe.ibm.com/ibmdl/export/pub/systems/cloud/runtimes/nodejs/"; \
    wget -q -U UA_IBM_Node_Docker -O /tmp/ibm-node.bin $BASE_URL/$BINARY; \
    echo "$SHA  /tmp/ibm-node.bin" | sha256sum -c - || exit 1; \
    mkdir -p /opt/ibm; \
    chmod +x /tmp/ibm-node.bin; \
    /tmp/ibm-node.bin -i silent  -DLICENSE_ACCEPTED=true -DUSER_INSTALL_DIR="/usr/local"; \
    apt-get purge -y --auto-remove $buildDeps;

ENV NODE_HOME=/usr/local/bin/node/bin/ \
    PATH=/usr/local/bin/node/bin:$PATH

ONBUILD RUN echo 'Please note that by using this image you are accepting the license terms.' \
            'Licenses are available at /usr/local/bin/node/license/'
