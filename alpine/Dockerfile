FROM alpine:3.11
LABEL maintainer="containers@vividcortex.com"
LABEL app=vividcortex

# How to use:
## docker build --force-rm --build-arg -t vcimage https://raw.githubusercontent.com/VividCortex/docker/master/alpine/Dockerfile
#opt1# default hostname : docker run --env VC_API_TOKEN=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx --detach --interactive --tty --name=vividcortex vcimage
#opt2# override hostname: docker run --env VC_API_TOKEN=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx --env VC_HOSTNAME=mycontainer --detach --interactive --tty --name=vividcortex vcimage
#opt3# monitor RDS      : docker run --env VC_API_TOKEN=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx --env VC_HOSTNAME=mycontainer --env VC_DRV_MANUAL_HOST_URI=mysql://user:pass@domain.xyz:3306/db --detach --interactive --tty --name=vividcortex vcimage
## docker ps | grep -q vividcortex && echo "success!"
## shell in container: docker exec --interactive --tty vividcortex /bin/sh
## REMOVE CONTAINER AND IMAGE: docker rm --force vividcortex; docker rmi --force vcimage

ENV VC_API_TOKEN NO

WORKDIR /
ENTRYPOINT /usr/local/bin/vc-agent-007 -foreground -forbid-restarts -api-token $VC_API_TOKEN

# openssl for https
# download, install
# cap logs

RUN apk --no-cache update && apk add --no-cache openssl ca-certificates wget && \
    rm -f install && \
    wget https://download.vividcortex.com/install && \
    sh install --token=${VC_API_TOKEN} --batch --init=None --static --proxy=dyn && \
    rm -f install && \
    sed -i '1 a \ \ \ \ "log-max-size":"5",' /etc/vividcortex/global.conf && \
    sed -i '1 a \ \ \ \ "log-max-backups":"1",' /etc/vividcortex/global.conf && \
    chmod 600 /etc/vividcortex/global.conf
