Docker image with HOL Light preinstalled
========================================

## Important

HOL Light with Multivariate Analysis **requires at least 2 (or more)
Gb** of memory!  It might be necessary to set an appropriate memory
limit in the docker configuration.

## Notes

- Based on Debian and use OCaml from the Debian distribution.
- Introduce an user account 'holuser' because dmtcp refuse to start
  under root.

## Targets of the multistage build

| Image tag         | Description                            |
|----------------   |-------------------------------------   |
| `holbox-base`     | Debian & updates + ocaml + holuser     |
| `holbox-build`    | Build tools + HOL Light + Dmtcp        |
| `hol-light-base`  | Only HOL Light installed               |
| `hol-light-ckpt`  | HOL Light + Dmtcp                      |

## Manually built images with checkpointed binaries

| Checkpoint                | Preloaded with                 |
|------------------------   |-----------------------------   |
| `hol-light-core`          | Core library                   |
| `hol-light-multivariate`  | Multivariate analysis          |
| `hol-light-complex`       | Complex analysis               |
| `hol-light-hypercomplex`  | Quaternions                    |

## How to build the images

Build and test HOL Light "base" (no Dmtcp, no checkpoint)
```
docker build --target hol-light-base -t hol-light-base .
```

if you want to use cache and already downloaded images, or use options
`--pull` and `--no-cache` redownload images and updates
```
docker build --pull --no-cache --target hol-light-base -t hol-light-base .
```
To run the container
```
docker run --rm -it hol-light-base
```

Build the other images (prepare checkpointing)
```
docker build --target holbox-build -t holbox-build hol-light-base
docker build --target hol-light-base -t hol-light-base hol-light-base
docker build --target hol-light-elpi -t hol-light-elpi hol-light-base
docker build --target hol-light-ckpt -t hol-light-ckpt hol-light-base
```

## Checkpoints

To start the container for building the checkpoints:
```
docker run --name build-ckpt -h holbox -it -m 4G holbox-build screen
```

## Frequently used commands

In one screen terminal
```
dmtcp_coordinator -q -p 7779
s
```

In another screen terminal (`C-a n`)
```
dmtcp_launch -q -j -p 7779 ocaml -init make.ml
```
Once finished loading (before checkpoint)
```
load_path := "/src" :: !load_path;;
Gc.compact();;
```

Back into the first terminal (`C-a n`)
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
## Build checkpoint images

Commit the build-ckpt container into an image called holbox-ckpt
```
docker container commit build-ckpt holbox-ckpt
```

Build the images with the ckeckpointed binaries
```
docker build --target hol-light-core -t hol-light-core hol-light-core
docker build --target hol-light-multivariate -t hol-light-multivariate hol-light-core
docker build --target hol-light-complex -t hol-light-complex hol-light-core
docker build --target hol-light-hypercomplex -t hol-light-hypercomplex hol-light-core
```

## Run the ckeckpointed images
```
docker container run --rm -h holbox -it hol-light-core
docker container run --rm -h holbox -it hol-light-multivariate
docker container run --rm -h holbox -it hol-light-complex
docker container run --rm -h holbox -it hol-light-hypercomplex
```
