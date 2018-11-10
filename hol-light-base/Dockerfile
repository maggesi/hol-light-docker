### ===========================================================================
### Dockerfile for a debian image with HOL Light preinstalled.
### Marco Maggesi (University of Florence, Italy)
### ===========================================================================

### ---------------------------------------------------------------------------
### Basic setup.
### ---------------------------------------------------------------------------

FROM debian:stretch

RUN apt-get update && apt-get -y upgrade

RUN apt-get -y install git-core ocaml-base-nox camlp5 make wget unzip

### ---------------------------------------------------------------------------
### Add a new user for HOL Light.
### ---------------------------------------------------------------------------

RUN adduser holuser --disabled-password --gecos ""

WORKDIR /home/holuser

USER holuser

### ---------------------------------------------------------------------------
### Install HOL Light (currently version 2018-11-05)
### ---------------------------------------------------------------------------

ARG COMMIT=ef1b0d4e58112bb1482abad3d1f4e2e308b729b0

RUN wget https://github.com/jrh13/hol-light/archive/$COMMIT.zip && \
    unzip $COMMIT.zip && rm $COMMIT.zip && mv hol-light-$COMMIT hol-light

WORKDIR hol-light

RUN make

### ---------------------------------------------------------------------------
### Cleanup.
### ---------------------------------------------------------------------------

USER root

RUN apt-get -y remove --purge git-core make wget unzip && \
    apt -y autoremove

USER holuser

### ---------------------------------------------------------------------------
### Entrypoint and default parameters.
### ---------------------------------------------------------------------------

ENTRYPOINT ["bash"]

CMD ["-c", "ocaml -I `camlp5 -where` -init make.ml"]
