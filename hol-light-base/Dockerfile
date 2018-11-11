### ===========================================================================
### Dockerfile for a debian image with HOL Light preinstalled.
### Marco Maggesi (University of Florence, Italy)
### ===========================================================================

### ---------------------------------------------------------------------------
### Basic setup.
### ---------------------------------------------------------------------------

FROM debian:stretch AS debian

RUN apt-get update && apt-get -y upgrade

### ---------------------------------------------------------------------------
### Add a new user for HOL Light.
### ---------------------------------------------------------------------------

FROM debian AS holbox-base

RUN apt-get -y install ocaml-base-nox camlp5

RUN adduser holuser --disabled-password --gecos ""

WORKDIR /home/holuser

### ---------------------------------------------------------------------------
### Install HOL Light (currently version 2018-11-05)
### ---------------------------------------------------------------------------

FROM holbox-base AS holbox-build

RUN apt-get -y install git-core make wget unzip

USER holuser

ARG COMMIT=ef1b0d4e58112bb1482abad3d1f4e2e308b729b0

RUN wget https://github.com/jrh13/hol-light/archive/$COMMIT.zip && \
    unzip $COMMIT.zip && rm $COMMIT.zip && mv hol-light-$COMMIT hol-light

WORKDIR hol-light

RUN make

### ---------------------------------------------------------------------------
### HOL Light base optimized image
### ---------------------------------------------------------------------------

FROM holbox-base

COPY --from=holbox-build /home/holuser/hol-light /home/holuser/hol-light

USER holuser

WORKDIR /home/holuser/hol-light

ENTRYPOINT ["bash"]

CMD ["-c", "ocaml -I `camlp5 -where` -init make.ml"]
