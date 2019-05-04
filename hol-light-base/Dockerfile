### ===========================================================================
### Dockerfile for a debian image with HOL Light preinstalled.
### Marco Maggesi (University of Florence, Italy)
### ===========================================================================

### ---------------------------------------------------------------------------
### Target: holbox-base
### Debian image with opam and minimal software installed (OCaml).
### ---------------------------------------------------------------------------

FROM ocaml/opam2:debian-stable AS holbox-base

USER root

RUN apt-get update && \
    apt-get -y install rlwrap

USER opam

RUN opam update && \
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
RUN apt-get -y install build-essential curl make screen tmux

USER opam

## --------------------------------------------------
## Install Dmtcp (version 2018-11-12)

RUN mkdir -p src/dmtcp

WORKDIR src/dmtcp

# ARG DMTCP_VERSION=99c5941fe7465c614cd2971de554cc7a81280d79
# Version 2019-02-22
ARG DMTCP_VERSION=8b94c398881073a6c597d2495a0041301b494352
# Version 2019-04-22
ARG DMTCP_VERSION=cfe168e2539b60e29bbac27da9a8b78b77add2a6

RUN curl -sL https://github.com/dmtcp/dmtcp/archive/$DMTCP_VERSION.tar.gz | tar xz --strip-components=1

RUN ./configure --prefix=/usr/local

RUN make -j 2

USER root

RUN make install

USER opam

## --------------------------------------------------
## Install HOL Light

WORKDIR /home/opam

USER opam

RUN mkdir -p hol-light

WORKDIR hol-light

# Version 2018-11-12
# ARG HOL_LIGHT_VERSION=9a22c3609a43933c654b15aeeb4d549a25918b71
# Version 2018-12-09
# ARG HOL_LIGHT_VERSION=4a2bda88c305b77dc78164466c38bb2c4075c635
# Version 2018-12-09
# ARG HOL_LIGHT_VERSION=6c7df495cd1538bc5956ce004e64e73b071ab22d
# Version 2019-02-17
# ARG HOL_LIGHT_VERSION=f6c2710061c3e7f108924194073d0f9b7d8814c3
# Version 2019-04-22
ARG HOL_LIGHT_VERSION=f6c2710061c3e7f108924194073d0f9b7d8814c3
# 2a6c5c210af7f7a6cd90c77d99740fc50fd3cb9c


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
RUN apt-get -y install screen tmux
USER opam

COPY --from=holbox-build /usr/local/ /usr/local/

ENTRYPOINT [""]
CMD ["bash"]
