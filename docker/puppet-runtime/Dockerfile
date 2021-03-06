ARG centos_version=7

FROM centos:${centos_version}

ARG vcs_ref
ARG build_date
ARG version="201904091"
ARG project="agent"
ARG project_branch="master"
ARG artifactory=https://artifactory.delivery.puppetlabs.net/artifactory/rpm__local/development/puppet-runtime/${version}/el-7-x86_64
ARG file_base=${project}-runtime-${project_branch}-${version}.el-7-x86_64
# validate_downloads.sh consumes these values
ARG RUNTIME_FILE="${file_base}.tar.gz"
ARG SETTINGS_FILE="${file_base}.settings.yaml"

LABEL org.label-schema.maintainer="Puppet Release Team <release@puppet.com>" \
      org.label-schema.vendor="Puppet" \
      org.label-schema.url="https://github.com/puppetlabs/puppet-runtime" \
      org.label-schema.license="Apache-2.0" \
      org.label-schema.vcs-url="https://github.com/puppetlabs/puppet-runtime" \
      org.label-schema.schema-version="1.0" \
      org.label-schema.dockerfile="/Dockerfile"

COPY validate_downloads.sh /runtime/validate_downloads.sh
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# hadolint ignore=DL3020
ADD ${artifactory}/${RUNTIME_FILE} \
    ${artifactory}/${RUNTIME_FILE}.sha1 \
    ${artifactory}/${SETTINGS_FILE} \
    ${artifactory}/${SETTINGS_FILE}.sha1 \
    /

LABEL org.label-schema.name="$project Runtime, $project_branch branch" \
      org.label-schema.version="$version" \
      org.label-schema.vcs-ref="$vcs_ref" \
      org.label-schema.build-date="$build_date"

RUN chmod +x /runtime/validate_downloads.sh && \
    /runtime/validate_downloads.sh && \
    gunzip -c ${file_base}.tar.gz | tar -k -C / -xf -

COPY Dockerfile /
