ARG FROM_IMAGE=ubuntu:16.04
FROM ${FROM_IMAGE}

ARG DV_GPU_BUILD=0
ENV DV_GPU_BUILD=${DV_GPU_BUILD}

ADD README.txt LICENSE AUTHORS /

RUN mkdir -p /tmp/deepvariant_docker && mkdir -p /opt/deepvariant/bin
ADD settings.sh run-prereq.sh *.whl /tmp/deepvariant_docker/
RUN cd /tmp/deepvariant_docker && ./run-prereq.sh && \
    rm -rf /tmp/deepvariant_docker/ && \
    apt-get install -y parallel

ADD make_examples.zip \
    call_variants.zip \
    postprocess_variants.zip \
    model_train.zip \
    model_eval.zip \
    /opt/deepvariant/bin/

# Create shell wrappers for python zip files for easier use.
RUN \
    BASH_HEADER='#!/bin/bash' && \
    printf "%s\n%s\n" \
        "${BASH_HEADER}" \
        'python /opt/deepvariant/bin/make_examples.zip "$@"' > \
        /opt/deepvariant/bin/make_examples && \
    printf "%s\n%s\n" \
        "${BASH_HEADER}" \
        'python /opt/deepvariant/bin/call_variants.zip "$@"' > \
        /opt/deepvariant/bin/call_variants && \
    printf "%s\n%s\n" \
        "${BASH_HEADER}" \
        'python /opt/deepvariant/bin/postprocess_variants.zip "$@"' > \
        /opt/deepvariant/bin/postprocess_variants && \
    printf "%s\n%s\n" \
        "${BASH_HEADER}" \
        'python /opt/deepvariant/bin/model_train.zip "$@"' > \
        /opt/deepvariant/bin/model_train && \
    printf "%s\n%s\n" \
        "${BASH_HEADER}" \
        'python /opt/deepvariant/bin/model_eval.zip "$@"' > \
        /opt/deepvariant/bin/model_eval && \
    chmod +x /opt/deepvariant/bin/make_examples \
             /opt/deepvariant/bin/call_variants \
             /opt/deepvariant/bin/postprocess_variants \
             /opt/deepvariant/bin/model_train \
             /opt/deepvariant/bin/model_eval
