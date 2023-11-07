# ROS Bridge Noetic <-> Foxy

## Assumptions
- Bridges ROS traffic between ROS Noetic and ROS Foxy
- All ROS nodes (ROS1 and ROS2) are running on the same machine
- All ROS nodes are being run as a non-root user
- DDS shared memory is not disabled by a custom profile
- Only the following message packages are bridged:
  - `actionlib_msgs`
  - `std_msgs`
  - `sensor_msgs`
  - `control_msgs`
  - `rosgraph_msgs`
  - `trajectory_msgs`
  - `tf2_msgs`
  - `geometry_msgs`

## Create the Docker image
### Download
Download a pre-built Docker image for this bridge from the container registry:

```
docker login ghcr.io
docker pull ghcr.io/ros-industrial/ros1_bridge:noetic-foxy
```

### Build
Build the Docker image using `docker-compose`:

```commandLine
cd ros1_bridge/docker/noetic_foxy
docker compose build
```

## Run
Run the ROS bridge with the following command:

```commandLine
cd ros1_bridge/docker/noetic_foxy
CURRENT_UID=$(id -u):$(id -g) docker compose up
```

Once your application is done, stop the brdige and remove the container using the following command:

```commandLine
cd ros1_bridge/docker/noetic_foxy
docker compose down
```
