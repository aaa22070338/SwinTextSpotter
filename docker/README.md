
## Use the container (with docker ≥ 19.03)

```
cd docker/
# Build:
docker build --build-arg USER_ID=1000 -t detectron2:v0 .
# Launch (require GPUs):
docker run --gpus all -it \
  --shm-size=8gb --env="DISPLAY" --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
  --volume="path/to/your/SwinTextSpotter/:/home/appuser/SwinTextSpotter"
  --name=SwinTextSpotter detectron2:v0

# Grant docker access to host X server to show images
xhost +local:`docker inspect --format='{{ .Config.Hostname }}' detectron2`
```
# Exit container:
```bash
exit
```


# start container
```bash
docker start SwinTextSpotter
dcoker exec -it SwinTextSpotter bash
```
# stop container
```bash
docker stop SwinTextSpotter
```
or use docker-desktop to stop it
