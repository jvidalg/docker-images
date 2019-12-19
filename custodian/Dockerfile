FROM python:3.7-slim-stretch

ARG VCS_REF
ARG BUILD_DATE

LABEL name="custodian" \
      description="custodian + aws"


ENV AWS_CLI_VERSION=1.16.190
ENV AWS_S3CMD_VERSION=2.0.1

# Transfer Custodian source into container by directory
# to minimize size

WORKDIR /src

RUN adduser --disabled-login custodian
RUN apt-get --yes update \
 && apt-get --yes install build-essential --no-install-recommends \
 && pip3 install c7n \
 && pip install --upgrade awscli==${AWS_CLI_VERSION} s3cmd==${AWS_S3CMD_VERSION} python-magic

USER custodian
WORKDIR /home/custodian
ENV LC_ALL="C.UTF-8" LANG="C.UTF-8"
VOLUME ["/home/custodian"]
#ENTRYPOINT ["/usr/local/bin/custodian"]
#CMD ["--help"]
