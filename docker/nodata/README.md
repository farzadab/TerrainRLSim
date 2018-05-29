# Docker image to compile everything from scratch

Instructions:
  - install docker: [official instructions](https://docs.docker.com/install/)
  - build:
```bash
docker build -t terrainrlnodata .
```
  - usage (**not the safest way to do this!**):
```bash
## gives access to host X11 for running GUI applications (obviously I trust the application, but you might not)
docker run -it --net=host --env="DISPLAY" --volume="$HOME/.Xauthority:/root/.Xauthority:rw" terrainrlnodata
## or just simply run it like this without the GUI
docker run -it terrainrlnodata
```
  - test:
```bash
## while inside the running container, run:
python3 simAdapter/terrainRLSimTest.py
## or
./TerrainRL -arg_file= args/biped3D/test_biped_3d_args.txt
```