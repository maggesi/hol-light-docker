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

## Targets of the multistage build

| Name              | Description                   |
|----------------   |----------------------------   |
| `debian`          | Debian with updates.          |
| `holbox-base`     | Add user for HOL Light.       |
| `holbox-build`    | Run the build of HOL Light.   |
| (unnamed)         | The final stage.              |
