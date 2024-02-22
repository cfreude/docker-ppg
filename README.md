## Practical Path Guiding - Docker

This repo enables to run PPG in a docker container and is supposed to be added as a submodule to an existing repo
via: ```git submodule add https://github.com/cfreude/docker-ppg docker```

At the start of the container all files in the ```mitsuba``` folder are copied to and override the corresponding files in the parent repo's ```mitsuba``` folder.

```
Note: Currently the Python API is disabled via "SConscript.configure".
```

We use a dockerfile with **docker compose** to build an image (based on **ubuntu 18.04**) which includes all necessary libraries and ENV variables.
The **Mitsuba's** source code is included by mounting the root folder of the parent repository as a **docker volume** into the folder "/code" inside the container.
A advantage of mounting the repo as a volume it, that all files and changes are synced.
**Mitsuba** is not compiled while building the **docker image**, but can be done manually inside the running **docker container**.

#### Docker Setup
1. Install [Docker](https://docs.docker.com/get-docker/) and start it.
2. Open a terminal.
3. Change to the root folder of this repository.
4. Execute ...
   1. for command-line: ```docker compose -f ./docker/dockerfiles/cmdline/docker-compose.yml up --build -d```
   2. for VNC: ```docker compose -f ./docker/dockerfiles/vnc/docker-compose.yml up --build -d```

Now the container is up and running, kept alive by the last CMD (```tail -F anything```) of the Dockerfile.
Next we need to build Mitsuba inside the container.
For this one can use e.g. ```docker exec``` or ```docker attach```.

#### Build and Run

1. Open a terminal.
2. Execute ...
   1. for command-line: ```docker exec -it misuba-ppg-cmdline bash``` to open an interactive shell inside the docker container.
   1. for VNC: [http://localhost:6080/](http://localhost:6080/)
3. Execute ```cd mitsuba/ && ./build.sh``` to build Mitsuba.
4. Execute ```./dist/mitsuba``` to test if Mitsuba runs.

#### Visual Studio Code

It is possible to attach Visual Studio Code directly to the running container. 

1. Install and open [Visual Studio Code](https://code.visualstudio.com/).
2. Install the [Visual Studio Code Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) Extension.
3. Press **Ctrl+Shift+P** and type or choose ```Dev Containers: Attach to Running Container...```.
4. Select ```/mitsuba``` container to open a new attached VS Code Editor Window.