# Get alpine jdk image
FROM openjdk:12-alpine

# Install wget
RUN apk add --no-cache wget

# Download server jar & accept eula
RUN wget "https://launcher.mojang.com/v1/objects/0a68a89351325fbad9b432b3eb24ae944860350e/server.jar" -O server.jar && echo "eula=true" > eula.txt

# Expose port
EXPOSE 25565

# Start Server
CMD java -Xms512M -Xmx1G -jar minecraft/server.jar nogui
