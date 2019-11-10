FROM debian:latest

# Set working directory
WORKDIR /home

# Install bzip lib & CA Certs
RUN apt-get update && apt-get install -y \
	bzip2 \
	libbz2-dev \
	ca-certificates \
 && rm -rf /var/lib/apt/lists/*

# Get TeamSpeak 3.9.1 Server
ADD https://files.teamspeak-services.com/releases/server/3.9.1/teamspeak3-server_linux_amd64-3.9.1.tar.bz2 /home

# Setup TeamSpeak Server
RUN	tar -jxvf teamspeak3-server_linux_amd64-3.9.1.tar.bz2 \
 && rm -rf teamspeak3-server_linux_amd64-3.9.1.tar.bz2

# Accept License Agreement
ENV TS3SERVER_LICENSE accept

# Expose ports
EXPOSE 9987/udp 10011 30033

# Start TeamSpeak Server
CMD "/home/teamspeak3-server_linux_amd64/ts3server_minimal_runscript.sh"

