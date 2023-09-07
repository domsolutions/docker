ARG BASE_IMAGE=domriordan/dom-go:v1.15.15
FROM ${BASE_IMAGE}

LABEL description="Image for plugin development"

ENV TYK_GW_PATH=/go/src/github.com/TykTechnologies/tyk

ENV GO111MODULE=on

# This directory will contain the plugin source and will be
# mounted from the host box by the user using docker volumes
ENV PLUGIN_SOURCE_PATH=/plugin-source

RUN mkdir -p $TYK_GW_PATH $PLUGIN_SOURCE_PATH

RUN apt-get update
RUN apt-get install -y git
RUN rm /usr/bin/passwd && rm /usr/sbin/adduser

ARG TYK_VERSION

RUN git clone https://github.com/TykTechnologies/tyk.git && rm -rf $TYK_GW_PATH && mv tyk /go/src/github.com/TykTechnologies
RUN cd $TYK_GW_PATH && git checkout tags/$TYK_VERSION
RUN cd $TYK_GW_PATH && go mod vendor

##ADD go.mod go.sum $TYK_GW_PATH
#WORKDIR $TYK_GW_PATH
#
#RUN go mod download
##RUN --mount=type=cache,mode=0755,target=/go/pkg/mod go mod download
#
##ADD . $TYK_GW_PATH
#
#ARG GITHUB_SHA
#ARG GITHUB_TAG
#ENV GITHUB_SHA ${GITHUB_SHA}
#ENV GITHUB_TAG ${TYK_VERSION}


COPY build.sh /build.sh
RUN chmod +x /build.sh

# for debugging
#ENTRYPOINT ["tail"]
#CMD ["-f","/dev/null"]
#
ENTRYPOINT ["/build.sh"]