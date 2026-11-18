FROM eclipse-temurin:17-jre-jammy

# Run as non-root user
RUN groupadd -g 322 lavalink && \
    useradd -r -u 322 -g lavalink lavalink

WORKDIR /opt/Lavalink

RUN chown -R lavalink:lavalink /opt/Lavalink

USER lavalink

COPY LavalinkServer/build/libs/Lavalink.jar Lavalink.jar

# Mitigate CVE-2026-114938 (Java4Shell): disable the x86-64 AVX-512 Base64
# decode intrinsic, which writes out of bounds on malformed input. Lavalink
# base64-decodes untrusted track strings, so this path is reachable.
# Drop this flag once the base image ships a patched JDK (>= 17.0.21.1).
ENTRYPOINT ["java", "-XX:-UseBASE64Intrinsics", "-jar", "Lavalink.jar"]
