# LAD analysis containers

This repository contains Apptainer definition files and build scripts for an
AlmaLinux 9 container with HCANA and LADlib. The optional GEANT4 definition
adds CLHEP and GEANT4 on top of the base image.

## Requirements

- [Apptainer](https://apptainer.org/)
- Network access during builds to download packages and source code

## Build the base image

From the repository directory, build the base image:

```sh
apptainer build lad-al9.sif al9.def
```

The definition installs the packages listed in `packages`, builds HCANA from
the `develop` branch.

Alternatively, download the latest published base image:

```sh
wget https://github.com/Hugo-Leung/lad-container/releases/latest/download/lad-al9.sif
```

## Add GEANT4

`geant4.def` uses `lad-al9.sif` as its base image. With that image in the
current directory, build the extended image:

```sh
apptainer build lad-al9-geant4.sif geant4.def
```

This layer builds CLHEP 2.4.7.1 and GEANT4 v11.4.0, including GEANT4 data.

## Use an image

Start an interactive shell:

```sh
apptainer shell lad-al9.sif
```

On ifarm, bind `/w` and `/cache` to make those host directories available
inside the container:

```sh
apptainer shell -B /w,/cache lad-al9.sif
```

Run a command in the container:

```sh
apptainer exec -B /w,/cache lad-al9.sif <command>
```

In the base image, `%runscript` invokes `hcana` and forwards any arguments
passed to `apptainer run`:

```sh
apptainer run -B /w,/cache lad-al9.sif [arguments...] <replay script>
```

## Repository files

- `al9.def` and the shell scripts build the base HCANA/LADlib image.
- `geant4.def`, `clhep.sh`, and `geant4.sh` build the optional GEANT4 layer.
- `packages` lists the AlmaLinux packages installed in the base image.
