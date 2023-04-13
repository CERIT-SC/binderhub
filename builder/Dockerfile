FROM gcr.io/kaniko-project/executor:debug as kexec
FROM busybox:musl as busybox
FROM scratch as scr

FROM quay.io/jupyterhub/repo2docker:2022.10.0

COPY --from=gcr.io/kaniko-project/executor:debug /kaniko/executor /kaniko/executor

RUN --mount=from=busybox,dst=/usr/ ["busybox", "sh", "-c", "mkdir -p /kaniko && chmod 777 /kaniko"] 

COPY --from=kexec /kaniko /kaniko
COPY --from=busybox /bin /busybox
# Declare /busybox as a volume to get it automatically in the path to ignore
VOLUME /busybox

RUN wget https://raw.githubusercontent.com/GoogleContainerTools/kaniko/main/files/nsswitch.conf -P /etc
RUN pip install pykaniko

ENV HOME /root
ENV USER root
ENV PATH "$PATH:/usr/local/bin:/kaniko:/busybox"
ENV SSL_CERT_DIR=/kaniko/ssl/certs
ENV DOCKER_CONFIG /kaniko/.docker/
ENV DOCKER_CREDENTIAL_GCR_CONFIG /kaniko/.config/gcloud/docker_credential_gcr_config.json
WORKDIR /workspace
RUN ["/busybox/mkdir", "-p", "/bin"]
RUN ["/busybox/ln", "-f", "-s", "/busybox/sh", "/bin/sh"]

COPY docker.py app.py __main__.py /usr/lib/python3.9/site-packages/repo2docker/
COPY base.py /usr/lib/python3.9/site-packages/repo2docker/buildpacks/
COPY kaniko.py /usr/lib/python3.9/site-packages/kaniko/ 

ENTRYPOINT ["jupyter-repo2docker", "--no-run"]
