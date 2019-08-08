Preparing a Docker container with OCaml and Dmtcp for HOL Light
===============================================================

## Important

HOL Light with Multivariate Analysis **requires at least 7 Gb** (maybe more)
of memory!  It might be necessary to set an appropriate memory limit in the
docker configuration.  See the Docker documentation.

## Notes

- Based on Debian and use OCaml from the Debian distribution.
- Dmtcp refuse to start under root: we run it under "opam" user.

## Step 1: Build the Docker image.

Build the image with the command:
```
docker build --target ocaml-dmtcp-elpi -t holelpi hol-light-base
```
if you want to use cache and the already downloaded images, or use options
`--pull` and `--no-cache` to redownload the images and clear the cache:
```
docker build --pull --no-cache --target hol-ready -t hol-ready hol-light-base
```

## Step 2: Test the image

To run a freshly created container

1. Go to the HOL Light directory:
   ```cd ~/where-your-hol-light-installation-is```
2. Start the container:
   ```
   docker run --rm -it -v "$(pwd):/home/opam/work" holelpi
   ```
This will open a bash session.
Then run

## Checkpoints

To start the container for building the checkpoints:
```
docker run --name holelpi -h holelpi -it \
           -v "$(pwd):/home/opam/work" holelpi screen
```

## Frequently used commands

In one screen terminal
```
dmtcp_coordinator -q -p 7779
s
```

In another screen terminal (use `C-a c` to open a new screen terminal)
```
dmtcp_launch -q -j -p 7779 ocaml -I `camlp5 -where` -init make.ml
```
Once finished loading (before checkpoint)
```
load_path := "/src" :: !load_path;;
Gc.compact();;
```

Back into the first terminal (`C-a n` to cycle between screen terminals)
```
c
```

Then open another screen tab and move the checkpoint in a separate
directory:
```
mkdir ckpt_core
mv ckpt*.dmtcp dmtcp_restart_script* ckpt_core
```

Next, load multivariate analysis
```
loadt "Multivariate/make.ml";;
Gc.compact();;
```

Then checkpoint and move in a separate directory
```
mkdir ckpt_multivariate
mv ckpt*.dmtcp dmtcp_restart_script* ckpt_multivariate
```

Load multivariate-based complex analysis
```
loadt "Library/binomial.ml";;
loadt "Multivariate/complexes.ml";;
loadt "Multivariate/canal.ml";;
loadt "Multivariate/transcendentals.ml";;
loadt "Multivariate/realanalysis.ml";;
loadt "Multivariate/moretop.ml";;
loadt "Multivariate/cauchy.ml";;
loadt "Multivariate/complex_database.ml";;
Gc.compact();;
```

Then checkpoint and move in a separate directory
```
mkdir ckpt_complex
mv ckpt*.dmtcp dmtcp_restart_script* ckpt_complex
```

Load quaternionic calculus
```
loadt "Quaternions/make.ml";;
Gc.compact();;
```

Then checkpoint and move in a separate directory
```
mkdir ckpt_hypercomplex
mv ckpt*.dmtcp dmtcp_restart_script* ckpt_hypercomplex
```
