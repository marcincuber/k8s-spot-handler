ARG BASE_IMAGE
FROM ${BASE_IMAGE}

ENV HOME=/srv
WORKDIR /srv

ARG VERSION
RUN apk add --no-cache curl ca-certificates
RUN curl -f -s -o /usr/local/bin/kubectl https://storage.googleapis.com/kubernetes-release/release/v${VERSION}/bin/linux/amd64/kubectl && \
    chmod +x /usr/local/bin/kubectl && \
    kubectl version --client

# Copy entrypoint.sh
COPY entrypoint.sh .

# Set permissions on the file.
RUN chmod +x entrypoint.sh


CMD ["/srv/entrypoint.sh"]
