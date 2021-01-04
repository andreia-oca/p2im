# Testing using `docker`

**NOTE:** Run the following commands on the **host machine** to stop AFL from
complaining:
```
su -
echo core >/proc/sys/kernel/core_pattern
# d /sys/devices/system/cpu
echo performance | tee cpu*/cpufreq/scaling_governor
exit
```

The `Dockerfile` provided builds an image with all the required dependencies to run P2IM.
The `Dockerfile` can be found in the `docker/` folder.
Inside the `docker/` folder the following files:

* The `Dockerfile` that builds the image
* A `Makefile` for convenience
* A configuration file to run P2IM on a real-world firmware named "Console"

To build the image navigate to the `docker/` directory and run `make`:
```
$ cd path/to/p2im/docker/
$ make
```

This will build the docker image locally.
View the available images with:
```
$ docker image ls
```

To start a self-destructing container that runs a simple test, run:
```
$ make test
```

This will run P2IM on a precompiled, real-world firmware provided in 
the repo at `externals/p2im-real_firmware/binary`.

To start a long running container with an interactive `/bin/bash` session, run:
```
$ make run
```

To connect to an existing, long running container, run:
```
$ make bash
```

To delete the container and local image crated, run:
```
$ make clean
```
