ARG HDTCPP_GIT_TAG="develop"

FROM alpine:3.20 AS builder

ARG HDTCPP_GIT_REPO="https://github.com/rdfhdt/hdt-cpp.git"
ARG HDTCPP_GIT_TAG="develop"
ARG BUILD_DEPS="build-base cmake git zlib-dev bzip2-dev curl-dev bash autoconf automake libtool serd-dev"

WORKDIR /tmp
RUN apk --update upgrade &&\
    apk add --no-cache $BUILD_DEPS && \
    git clone --depth 1 --recursive --branch $HDTCPP_GIT_TAG $HDTCPP_GIT_REPO && \
    cd hdt-cpp && \
    sed -i '31i #include <cstdint>' libhdt/include/Iterator.hpp && \
    sed -i '31i #include <cstdint>' libhdt/include/RDFParser.hpp && \
    sed -i '31i #include <cstdint>' libhdt/include/Dictionary.hpp && \
    sed -i '31i #include <cstdint>' libhdt/include/RDF.hpp && \
    sed -i '31i #include <cstdint>' libhdt/include/RDFSerializer.hpp && \
    sed -i '31i #include <cstdint>' libhdt/include/SingleTriple.hpp && \
    ./autogen.sh && \
    ./configure && \
    make CXX_FLAGS="-include cstdint" -j$(nproc) && \
    make install && \
    ldd /usr/local/bin/rdf2hdt

FROM alpine:3.20

LABEL maintainer="cmahnke@gmail.com"
LABEL org.opencontainers.image.source="https://github.com/cmahnke/lod-tools"

ARG RUN_DEPS="libstdc++ libgcc zlib bzip2 libcurl serd-libs"

COPY --from=builder /usr/local/bin/rdf2hdt /usr/local/bin/
COPY --from=builder /usr/local/bin/hdt2rdf /usr/local/bin/
COPY --from=builder /usr/local/bin/hdtSearch /usr/local/bin/
COPY --from=builder /usr/local/lib/libhdt.so* /usr/local/lib/
COPY --from=builder /usr/local/lib/libcds.so* /usr/local/lib/


RUN apk add --no-cache $RUN_DEPS && \
    ldconfig /usr/local/lib

WORKDIR /data
ENTRYPOINT ["rdf2hdt"]
