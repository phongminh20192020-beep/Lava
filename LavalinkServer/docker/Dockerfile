# Stage 1: Build the jar
FROM gradle:8-jdk17 AS build
WORKDIR /build
COPY . .
RUN git init && \
    git add -A && \
    git -c user.email="build@railway.app" -c user.name="Railway" commit -m "build" --allow-empty
RUN gradle :LavalinkServer:bootJar --no-daemon

# Stage 2: Run the jar
FROM eclipse-temurin:17-jre-jammy
RUN groupadd -g 322 lavalink && \
    useradd -r -u 322 -g lavalink lavalink
WORKDIR /opt/Lavalink
RUN chown -R lavalink:lavalink /opt/Lavalink
USER lavalink
COPY --from=build /build/LavalinkServer/build/libs/Lavalink.jar Lavalink.jar
ENTRYPOINT ["java", "-jar", "Lavalink.jar"]
