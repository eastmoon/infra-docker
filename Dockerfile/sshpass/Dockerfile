FROM bash

# Install libaray
RUN \
    apk update
RUN \
    apk add \
        curl \
        rsync \
        openssh \
        sshpass

# Setting Container
ENTRYPOINT ["sshpass"]
