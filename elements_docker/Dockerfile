FROM debian:bullseye-slim
# aarch64, x86_64
ARG ARCH=x86_64

ENV ELEMENTS_SHORT_VERSION 0.17.0
ENV ELEMENTS_VERSION ${ELEMENTS_SHORT_VERSION}.2
ENV FILENAME elements-${ELEMENTS_VERSION}-${ARCH}-linux-gnu.tar.gz
ENV BASE_URL  https://github.com/ElementsProject/elements/releases/download/elements-${ELEMENTS_VERSION}
ENV ELEMENTS_URL ${BASE_URL}/${FILENAME}
ENV SHASUM_URL ${BASE_URL}/SHA256SUMS.asc

ENV PATH elements-${ELEMENTS_SHORT_VERSION}/bin:${PATH}

RUN apt-get update \
 && apt-get install -yy bash netcat wget \
 && mkdir /data \
 && wget "${SHASUM_URL}" \
 && wget "${ELEMENTS_URL}" \
 && apt-get remove -y wget \
 && rm -rf /var/lib/apt/lists/* \
 && grep ${FILENAME} SHA256SUMS.asc | sha256sum -c - \
 && rm SHA256SUMS.asc \
 && tar -zxf ${FILENAME} \
 && rm ${FILENAME} \
 && cd elements-${ELEMENTS_SHORT_VERSION}\
 && du -khs .

ADD entrypoint.sh entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
CMD ["elementsd", "-datadir=/data"]

