FROM debian:stretch-slim AS build

RUN apt-get update && apt-get install -y git build-essential cmake gcc git libpcre3-dev libssl1.0-dev libsnmp-dev linux-libc-dev

RUN git clone --branch 1.11.1 --depth 1  https://git.code.sf.net/p/accel-ppp/code /usr/src/accel-ppp

WORKDIR /usr/src/accel-ppp/build

RUN cmake \
        -DRADIUS=TRUE \
        -DNETSNMP=FALSE \
        -DCMAKE_INSTALL_PREFIX=/usr \
        ..

RUN make install


FROM debian:stretch-slim

RUN apt-get update && apt-get install -y libpcre3 libssl1.0.2 libsnmp30 ppp pppoe

COPY --from=build /usr/sbin/accel-pppd /usr/sbin/accel-pppd
COPY --from=build /usr/bin/accel-cmd /usr/bin/accel-cmd
COPY --from=build /usr/share/accel-ppp /usr/share/accel-ppp
COPY --from=build /usr/lib64/accel-ppp /usr/lib64/accel-ppp

ENTRYPOINT ["/usr/sbin/accel-pppd"]
CMD ["-c", "/etc/accel-ppp.conf"]
