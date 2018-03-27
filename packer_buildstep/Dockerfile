FROM debian:stable
RUN apt-get -y update && apt-get -y install ca-certificates \
                                            unzip \
                                            puppet \
                                            libffi-dev \
                                            python3-pip \
                                            && pip3 install ansible && rm -rf /var/lib/apt/lists/* && apt-get clean

ADD https://releases.hashicorp.com/packer/1.1.2/packer_1.1.2_linux_amd64.zip .
RUN echo '7e315a6110333d9d4269ac2ec5c68e663d82a4575d3e853996a976875612724b  packer_1.1.2_linux_amd64.zip' | sha256sum && /usr/bin/unzip packer_1.1.2_linux_amd64.zip -d /usr/bin
ENTRYPOINT ["/usr/bin/packer"]
