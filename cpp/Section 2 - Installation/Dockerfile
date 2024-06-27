FROM ubuntu:latest

RUN apt-get update && \
    apt-get install -y build-essential wget && \
    rm -rf /var/lib/apt/lists/*

RUN wget --quiet --content-disposition http://kayari.org/gcc-latest/gcc-latest.deb && \
    dpkg -i gcc-latest*.deb && \
    rm gcc-latest*.deb