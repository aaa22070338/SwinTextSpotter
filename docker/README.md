
## Use the container (with docker ≥ 19.03)

```
cd docker/
# Build:
docker build --build-arg USER_ID=1000 -t detectron2:v0 .
# Launch (require GPUs):
# please change the --volume path to your absolute repo clone directory
# Inside WSL 
docker run --gpus all -it \
  --shm-size=8gb --env="DISPLAY" --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
  --volume="path/to/your/SwinTextSpotter/:/home/appuser/SwinTextSpotter/" \     #if you are using wsl with /mnt/c/ prefix.  "C:/ prefix for powershell.
  --name=SwinTextSpotter detectron2:v0
# Or inside PowerShell

```
# Exit container:
```bash
exit
```


# start container
```bash
docker start SwinTextSpotter
docker exec -it SwinTextSpotter bash
```
# stop container
```bash
docker stop SwinTextSpotter
```
or use docker-desktop to stop it
