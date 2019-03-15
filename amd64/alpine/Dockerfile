FROM alpine:3.9 as builder
MAINTAINER wtakase <wataru.takase@kek.jp>

ENV G4_VERSION 10.05

RUN apk --no-cache add cmake curl expat-dev g++ gcc make && \
    mkdir -p /opt/geant4/src && \
    mkdir -p /opt/geant4/build && \
    mkdir -p /opt/geant4/install && \
    mkdir -p /opt/geant4/data && \
    curl -o /geant4.tar.gz http://geant4-data.web.cern.ch/geant4-data/releases/source/geant4.${G4_VERSION}.tar.gz && \
    tar xf /geant4.tar.gz -C /opt/geant4/src && \
    cd /opt/geant4/build && \
    cmake -DCMAKE_INSTALL_PREFIX=/opt/geant4/install \
          -DGEANT4_INSTALL_DATA=ON \
          -DGEANT4_INSTALL_DATADIR=/opt/geant4/data \
          -DGEANT4_BUILD_MULTITHREADED=ON \
          -DGEANT4_INSTALL_EXAMPLES=OFF \
          ../src/geant4.${G4_VERSION} && \
    make -j`nproc` && \
    make install


FROM alpine:3.9

RUN apk --no-cache add bash cmake expat-dev g++ gcc make && \
    mkdir -p /opt/geant4/data
COPY --from=builder /opt/geant4/install /opt/geant4/install
COPY entry-point.sh /entry-point.sh

WORKDIR /root

ENTRYPOINT ["/entry-point.sh"]
CMD ["/bin/bash"]