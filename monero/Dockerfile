FROM debian:bullseye-slim as builder

ARG monero_version="0.18.1.0"

RUN apt-get update && apt-get install -y bzip2 && \
    rm -rf /var/lib/apt/lists/*

RUN addgroup --gid 1337 moonramp
RUN adduser --disabled-password --shell /bin/sh moonramp --gid 1337 --uid 1337 --gecos ''
RUN mkdir /home/moonramp/.bitmonero
COPY ./monero.conf /home/moonramp/.bitmonero/monero.conf
RUN chown -R moonramp:moonramp /home/moonramp

WORKDIR /opt

COPY ./monero.sha256 /opt/sums

ADD https://downloads.getmonero.org/cli/monero-linux-x64-v$monero_version.tar.bz2 ./monero-v$monero_version-linux-x64.tar.bz2

RUN sha256sum -c sums
RUN tar -xjf monero-v$monero_version-linux-x64.tar.bz2 && cp -r /opt/monero-x86_64-linux-gnu-v$monero_version/* /opt/ && rm -rf monero-v$monero_version-linux-x64.tar.bz2 monero-x86_64-linux-gnu-v$monero_version

FROM scratch

COPY --from=builder /etc/passwd /etc/passwd
COPY --from=builder /home/moonramp /home/moonramp

ENV LOGNAME=moonramp
USER moonramp

COPY --from=builder /lib/x86_64-linux-gnu /lib/x86_64-linux-gnu/
COPY --from=builder /lib64 /lib64/
COPY --from=builder /bin/sh /bin/
COPY --from=builder /opt/monero-blockchain-ancestry /usr/bin/
COPY --from=builder /opt/monero-blockchain-depth /usr/bin/
COPY --from=builder /opt/monero-blockchain-export /usr/bin/
COPY --from=builder /opt/monero-blockchain-import /usr/bin/
COPY --from=builder /opt/monero-blockchain-mark-spent-outputs /usr/bin/
COPY --from=builder /opt/monero-blockchain-prune /usr/bin/
COPY --from=builder /opt/monero-blockchain-prune-known-spent-data /usr/bin/
COPY --from=builder /opt/monero-blockchain-stats /usr/bin/
COPY --from=builder /opt/monero-blockchain-usage /usr/bin/
COPY --from=builder /opt/monero-gen-ssl-cert /usr/bin/
COPY --from=builder /opt/monero-gen-trusted-multisig /usr/bin/
COPY --from=builder /opt/monero-wallet-cli /usr/bin/
COPY --from=builder /opt/monero-wallet-rpc /usr/bin/
COPY --from=builder /opt/monerod /usr/bin/

# mainnet
EXPOSE 18080
EXPOSE 18081
# testnet
EXPOSE 38080
EXPOSE 38081
# regtest
EXPOSE 48080
EXPOSE 48081

# monero-wallet-cli --regtest --restore-deterministic-wallet \
#   --electrum-seed="sequence atlas unveil summon pebbles tuesday beer rudely snake rockets different fuselage woven tagged bested dented vegan hover rapid fawns obvious muppet randomly seasons randomly" \
#   --password ""
# monero-wallet-cli --regtest --trusted-daemon

ENTRYPOINT ["monerod"]
CMD ["--version"]
