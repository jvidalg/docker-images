FROM alpine:3.10

ARG VCS_REF
ARG BUILD_DATE

# Metadata
LABEL org.label-schema.vcs-ref=$VCS_REF \
      org.label-schema.name="helm-kubectl" \
      org.label-schema.build-date=$BUILD_DATE

ENV KUBE_LATEST_VERSION="v1.16.2"
ENV HELM_VERSION="v3.0.0"
ENV TERRAFORM_VERSION=0.12.16
ENV AWS_CLI_VERSION=1.16.190
ENV AWS_S3CMD_VERSION=2.0.1
ENV PATH = $PATH:/usr/local/google-cloud-sdk/bin/

RUN apk add --no-cache ca-certificates bash git openssh curl \
    && wget -q https://storage.googleapis.com/kubernetes-release/release/${KUBE_LATEST_VERSION}/bin/linux/amd64/kubectl -O /usr/local/bin/kubectl \
    && chmod +x /usr/local/bin/kubectl \
    && wget -q https://get.helm.sh/helm-${HELM_VERSION}-linux-amd64.tar.gz -O - | tar -xzO linux-amd64/helm > /usr/local/bin/helm \
    && chmod +x /usr/local/bin/helm \
    && apk update \
    && apk add curl jq python bash ca-certificates git openssl unzip wget \
    && cd /tmp \
    && wget https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip \
    && unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip -d /usr/bin \
    && wget https://dl.google.com/dl/cloudsdk/channels/rapid/google-cloud-sdk.zip -O /tmp/google-cloud-sdk.zip \
    && cd /usr/local && unzip /tmp/google-cloud-sdk.zip \
    && google-cloud-sdk/install.sh --usage-reporting=false --path-update=true --bash-completion=true \
    && google-cloud-sdk/bin/gcloud config set --installation component_manager/disable_update_check true \
    && rm -rf /tmp/* \
    && rm -rf /var/cache/apk/* \
    && rm -rf /var/tmp/* \
    && apk -v --update add python py-pip groff less mailcap \
    && pip install --upgrade awscli==${AWS_CLI_VERSION} s3cmd==${AWS_S3CMD_VERSION} python-magic \
    && apk -v --purge del py-pip \
    && rm /var/cache/apk/*

WORKDIR /config

CMD bash
