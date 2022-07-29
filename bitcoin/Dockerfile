FROM debian:bullseye-slim as builder

ARG bitcoin_core_version="23.0"

RUN apt-get update && apt-get install -y bzip2 && \
    rm -rf /var/lib/apt/lists/*

RUN addgroup --gid 1337 moonramp
RUN adduser --disabled-password --shell /bin/sh moonramp --gid 1337 --uid 1337 --gecos ''
RUN mkdir /home/moonramp/.bitcoin
COPY ./bitcoin.conf /home/moonramp/.bitcoin/bitcoin.conf
RUN chown -R moonramp:moonramp /home/moonramp

WORKDIR /opt

COPY ./bitcoin_core.sha256 /opt/sums

ADD https://bitcoincore.org/bin/bitcoin-core-$bitcoin_core_version/bitcoin-$bitcoin_core_version-x86_64-linux-gnu.tar.gz ./bitcoin-$bitcoin_core_version-x86_64-linux-gnu.tar.gz

RUN sha256sum -c sums
RUN tar -xzf bitcoin-$bitcoin_core_version-x86_64-linux-gnu.tar.gz && cp -r /opt/bitcoin-$bitcoin_core_version/bin/* /opt/ && rm -rf /opt/bitcoin-$bitcoin_core_version-x86_64-linux-gnu.tar.gz bitcoin-$bitcoin_core_version

FROM scratch

COPY --from=builder /etc/passwd /etc/passwd
COPY --from=builder /home/moonramp /home/moonramp

ENV LOGNAME=moonramp
USER moonramp

COPY --from=builder /lib/x86_64-linux-gnu /lib/x86_64-linux-gnu
COPY --from=builder /lib64/ /lib64/
COPY --from=builder /bin/sh /bin/
COPY --from=builder /opt/bitcoin-cli /usr/bin/
COPY --from=builder /opt/bitcoin-qt /usr/bin/
COPY --from=builder /opt/bitcoin-tx /usr/bin/
COPY --from=builder /opt/bitcoin-util /usr/bin/
COPY --from=builder /opt/bitcoin-wallet /usr/bin/
COPY --from=builder /opt/bitcoind /usr/bin/
COPY --from=builder /opt/test_bitcoin /usr/bin/

# mainnet
EXPOSE 8332
# testnet
EXPOSE 18332
# signet
EXPOSE 38332
# regtest
EXPOSE 18444

# bitcoin-cli -regtest createwallet moonramp
# bitcoin-cli -regtest generatetoaddress 101 $(bitcoin-cli -regtest getnewaddress)

ENTRYPOINT ["bitcoind"]
CMD ["--version"]
