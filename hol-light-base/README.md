Docker image with HOL Light preinstalled
========================================

## Notes

	- Based on Debian and use OCaml from the Debian distribution.
	- Introduce an user account 'holuser'.

## How to build the image and run the container

To build the image:
```
docker build -t hol-light-base .
```

To run the container:
```
docker run --rm -it hol-light-base
```
