### ===========================================================================
### Dockerfile for a debian image with HOL Light preinstalled.
### Marco Maggesi (University of Florence, Italy)
### ===========================================================================

### ---------------------------------------------------------------------------
### Target: holbox-base
### Debian image with opam, updates and minimal software installed (OCaml).
### ---------------------------------------------------------------------------

FROM ocaml/opam2:debian-stable AS holbox-base

USER root

RUN apt-get update && \
    apt-get -y upgrade && \
    apt-get -y install rlwrap

USER opam

RUN opam update && opam upgrade && \
    opam switch create 4.04.1 ocaml-base-compiler.4.04.1 && \
    opam pin camlp5 6.17 && opam install camlp5

### ---------------------------------------------------------------------------
### Target: holbox-build
### Build an image with essential build tools added and with HOL Light and
### Dmtpc preinstalled.
### ---------------------------------------------------------------------------

FROM holbox-base AS holbox-build

USER root

# Screen is needed for manual checkpointing.
RUN apt-get -y install build-essential curl make screen

USER opam

## --------------------------------------------------
## Install Dmtcp (version 2018-11-12)

RUN mkdir -p src/dmtcp

WORKDIR src/dmtcp

ARG DMTCP_VERSION=99c5941fe7465c614cd2971de554cc7a81280d79

RUN curl -sL https://github.com/dmtcp/dmtcp/archive/$DMTCP_VERSION.tar.gz | tar xz --strip-components=1

RUN ./configure --prefix=/usr/local

RUN make -j 2

USER root

RUN make install

USER opam

## --------------------------------------------------
## Install HOL Light (currently version 2018-11-12)

WORKDIR /home/opam

USER opam

RUN mkdir -p hol-light

WORKDIR hol-light

ARG HOL_LIGHT_VERSION=9a22c3609a43933c654b15aeeb4d549a25918b71

RUN curl -sL https://github.com/jrh13/hol-light/archive/$HOL_LIGHT_VERSION.tar.gz | tar xz --strip-components=1

RUN eval `opam config env` && make

### ---------------------------------------------------------------------------
### Target: hol-light-base
### Image with HOL Light (and OCaml) installed and nothing else
### (no Dmtpc, no checkpoints).
### ---------------------------------------------------------------------------

FROM holbox-base AS hol-light-base

COPY --chown=opam:opam --from=holbox-build /home/opam/hol-light /home/opam/hol-light

USER opam

WORKDIR /home/opam/hol-light

CMD eval `opam config env` && ocaml -I `camlp5 -where` -init make.ml

### ---------------------------------------------------------------------------
### Target: hol-light-elpi
### Image with HOL Light (and OCaml) and Elpi (lambda-prolog
### interpreter) installed and nothing else (no Dmtpc, no
### checkpoints).
### ---------------------------------------------------------------------------

FROM hol-light-base AS hol-light-elpi

USER root

RUN apt-get install -y m4

USER opam

RUN eval `opam config env` && opam install -y elpi

WORKDIR /home/opam/hol-light

CMD eval `opam config env` && ocaml -I `camlp5 -where` -init make.ml

### ---------------------------------------------------------------------------
### Target: hol-light-ckpt
### Image with HOL Light (and OCaml) and Dmtcp (but no checkpoints).
### ---------------------------------------------------------------------------

FROM hol-light-base AS hol-light-ckpt

# Screen is needed for manual checkpointing.
USER root
RUN apt-get -y install screen
USER opam

COPY --from=holbox-build /usr/local/ /usr/local/

ENTRYPOINT [""]
CMD ["bash"]
