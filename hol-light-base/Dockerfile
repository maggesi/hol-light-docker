### ===========================================================================
### Dockerfile for a debian image with HOL Light preinstalled.
### Marco Maggesi (University of Florence, Italy)
### ===========================================================================

### ---------------------------------------------------------------------------
### Target: holbox-base
### Debian image with updates and minimal software installed (OCaml) and
### configurations (a dedicated "holuser").
### ---------------------------------------------------------------------------

FROM debian:stretch AS holbox-base

RUN apt-get update && apt-get -y upgrade

RUN apt-get -y install ocaml-base-nox camlp5

RUN adduser holuser --disabled-password --gecos ""

WORKDIR /home/holuser

### ---------------------------------------------------------------------------
### Target: holbox-build
### Build an image with essential build tools added and with HOL Light and
### Dmtpc preinstalled.
### ---------------------------------------------------------------------------

FROM holbox-base AS holbox-build

# Screen is needed for manual checkpointing.
RUN apt-get -y install build-essential curl make screen

## Install Dmtcp (version 2018-11-12)

RUN mkdir -p /src/dmtcp

WORKDIR /src/dmtcp

ARG DMTCP_VERSION=99c5941fe7465c614cd2971de554cc7a81280d79

RUN curl -sL https://github.com/dmtcp/dmtcp/archive/$DMTCP_VERSION.tar.gz | tar xz --strip-components=1

RUN ./configure --prefix=/usr/local

RUN make -j 2

RUN make install

## Install HOL Light (currently version 2018-11-12)

WORKDIR /home/holuser

USER holuser

RUN mkdir -p hol-light

WORKDIR hol-light

ARG HOL_LIGHT_VERSION=9a22c3609a43933c654b15aeeb4d549a25918b71

RUN curl -sL https://github.com/jrh13/hol-light/archive/$HOL_LIGHT_VERSION.tar.gz | tar xz --strip-components=1

RUN make

### ---------------------------------------------------------------------------
### Target: hol-light-base
### Image with HOL Light (and OCaml) installed and nothing else
### (no Dmtpc, no checkpoints).
### ---------------------------------------------------------------------------

FROM holbox-base AS hol-light-base

COPY --chown=holuser:holuser --from=holbox-build /home/holuser/hol-light /home/holuser/hol-light

USER holuser

WORKDIR /home/holuser/hol-light

## ENTRYPOINT ["bash"]
## CMD ["-c", "ocaml -I `camlp5 -where` -init make.ml"]

CMD ["ocaml -I `camlp5 -where` -init make.ml"]

### ---------------------------------------------------------------------------
### Target: hol-light-ckpt
### Image with HOL Light (and OCaml) and Dmtcp (but no checkpoints).
### ---------------------------------------------------------------------------

FROM hol-light-base AS hol-light-ckpt

# Screen is needed for manual checkpointing.
USER root
RUN apt-get -y install screen
USER holuser

COPY --from=holbox-build /usr/local/ /usr/local/

ENTRYPOINT [""]
CMD ["bash"]
