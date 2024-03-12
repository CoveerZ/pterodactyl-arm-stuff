FROM debian:stable-slim

ENV DEBIAN_FRONTEND noninteractive

# install dependencies

RUN dpkg --add-architecture armhf

RUN cp /bin/echo /usr/bin/systemctl

RUN apt-get update && \
    apt-get upgrade -y

RUN apt-get install -y gcc-arm-linux-gnueabihf libc6:armhf libncurses5:armhf libstdc++6:armhf

RUN apt-get install -y wget tar curl gcc g++ libgcc1 iproute2 gdb libsdl1.2debian libfontconfig1 telnet net-tools tzdata gnupg2 dirmngr ca-certificates libstdc++6 libc6

RUN apt-get install -y libtcmalloc-minimal4:armhf libnm0:armhf libcurl4-gnutls-dev:armhf libssl3:armhf libcurl4:armhf libtinfo6:armhf libncurses5:armhf libcurl3-gnutls:armhf libsdl2-2.0-0:armhf

# mono-complete

RUN gpg --homedir /tmp --no-default-keyring --keyring /usr/share/keyrings/mono-official-archive-keyring.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF && \
echo "deb [signed-by=/usr/share/keyrings/mono-official-archive-keyring.gpg] https://download.mono-project.com/repo/debian stable-buster main" | tee /etc/apt/sources.list.d/mono-official-stable.list

# box86

RUN wget https://itai-nelken.github.io/weekly-box86-debs/debian/box86.list -O /etc/apt/sources.list.d/box86.list && \
wget -qO- https://itai-nelken.github.io/weekly-box86-debs/debian/KEY.gpg | gpg --dearmor -o /etc/apt/trusted.gpg.d/box86-debs-archive-keyring.gpg

# box64

RUN wget https://ryanfortner.github.io/box64-debs/box64.list -O /etc/apt/sources.list.d/box64.list
RUN wget -qO- https://ryanfortner.github.io/box64-debs/KEY.gpg | gpg --dearmor -o /etc/apt/trusted.gpg.d/box64-debs-archive-keyring.gpg
 

RUN apt-get update && \
    apt-get install box86 box64-arm64 -y

RUN apt-get install mono-complete -y

RUN apt-get autoremove -y

RUN useradd -m -d /home/container -s /bin/bash container
RUN ln -s /home/container/ /nonexistent

ENV USER=container HOME=/home/container

COPY ./entrypoint.sh /entrypoint.sh

CMD ["/bin/bash", "/entrypoint.sh"]
