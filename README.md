# SAUV-Config

Contains Docker configs for running ROS2 Humble on our autonomy stack, SAUV-Autonomy. Please use the container to ensure a consistent development environment.

# Installation

1. On your Linux VM, install [Docker Desktop](https://docs.docker.com/desktop/install/linux-install/). If you're having trouble with installation like Miyu, install using the [apt repository](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository).

2. To pull the latest image, run: 
```
docker pull selenasun1618/ros2_humble:latest
```

3. Create & move the necessary folders so your folder structure is as follows:

```
SAUV/
|- SAUV-Autonomy/
    |- src/
    |- ...
|- SAUV-Config/
    |- Dockerfile.ros2_humble
    |- ...
```

4. Run the image: 
```
sudo docker run -it --name sauv-container --device /dev/i2c-7 -v /home/selenas/SAUV:/SAUV selenasun1618/ros2_humble:latest
```
    
Note: You need the `--device` flag to access the i2c bus for the IMU.

You should now see a Docker container running in the `SAUV` folder. Navigate to the `SAUV-Autonomy` folder to start developing.


# Usage

1. To pull the latest image, run: 
```
docker pull selenasun1618/ros2_humble:latest
```

2. To run the image in the first terminal, run the following command, changing `~/SAUV` to the path to your SAUV folder:
```
sudo docker run -it --name sauv_contianer --device=/dev/ttyUSB0 --device=/dev/i2c-7 -v ~/SAUV:/SAUV selenasun1618/ros2_humble:latest
```
To run other termainals in the same container (and the same ROS2 network), use the following command: 
```
docker exec -it [container name] bash
```

3. In every new terminal, run the following commands:
```
source /opt/ros/humble/setup.bash
source /SAUV/SAUV-Autonomy/install/setup.bash
```

# Updating the Docker image

If you added a new package to the image (in `Dockerfile.ros2_humble`), you need to update the image.

1. Rebuild the docker image: 
```
docker build -t selenasun1618/ros2_humble:latest .
```

Push the image to docker hub, after asking Selena to add you as a collaborator:

2. Log into Docker hub: 
```
docker login
```
3. Push the image: 
```
docker push selenasun1618/ros2_humble:latest
```