Docker Pentaho Data Integration
===============================

# Introduction

DockerFile for [Pentaho Data Integration](https://sourceforge.net/projects/pentaho/) (a.k.a kettel / PDI)

This image is intended to allow execution of PDI transformations and jobs through command line and run PDI's UI (`Spoon`). PDI server (`Carte`) is available on this image.

# Quick start

## Basic Syntax

```
$ docker container run --rm tgmti/pdi

Usage:	/entrypoint.sh COMMAND

Pentaho Data Integration (PDI)

Options:
  encr password		Obfuscate a password
  runj filename		Run job file
  runt filename		Run transformation file
  spoon			    Run spoon (GUI)
  help		         Print this help

```

## Running Transformations

```
$ docker container run --rm -v $(pwd):/jobs tgmti/pdi runt sample/dummy.ktr
```

## Running Jobs

```
$ docker container run --rm -v $(pwd):/jobs tgmti/pdi runj  sample/dummy.kjb
```

## Running Spoon (UI)

### Using `docker run`

```
$ docker run -it --rm -v /tmp/.X11-unix/:/tmp/.X11-unix/:ro \
        -v $(pwd):/jobs \
        -e XAUTH=$(xauth list|grep `uname -n` | cut -d ' ' -f5) -e "DISPLAY" \
        --name spoon \
        tgmti/pdi spoon
```

### Using startup script (Installing)

In order to run the container as if the application was installed locally, download the `spoon` script to a directory in you $PATH, for example:

```bash
$ sudo curl -fsSL https://raw.githubusercontent.com/tgmti/docker-pdi/master/spoon \
       -o /usr/local/bin/spoon
$ sudo chmod +x /usr/local/bin/spoon
```

Then you'll be able to run spoon in the current directory simply by calling `spoon`:

```bash
$ spoon
```


## Custom `kettle.properties`

In order to use a custom `kettle.properties`, you need to leave the file available in `/jobs/kettle.properties`.

```bash
$ # Custom properties in $(pwd)/kettle.properties
$ docker container run --rm -v $(pwd):/jobs tgmti/pdi runj  sample/dummy.kjb
```

# Environment variables

This image uses several environment variables in order to control its behavior, and some of them may be required

| Environment variable | Default value | Note |
| -------------------- | ------------- | -----|
| PDI\_VERSION | 9.1 | |
| |  | |

# Issues

If you have any problems with or questions about this image, please contact me
through a [GitHub issue](https://github.com/tgmti/docker-pdi/issues).
