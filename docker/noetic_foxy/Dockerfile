# syntax = docker/dockerfile:1.2

FROM ubuntu:20.04

ENV DEBIAN_FRONTEND noninteractive

SHELL ["/bin/bash", "-c"]

USER root

ADD .. /opt/ros1_bridge/src/ros1_bridge

RUN apt update \
  && apt upgrade -y \
  && apt install -y --no-install-recommends \
    apt-utils \
    build-essential \
    bash-completion \
    build-essential \
    ca-certificates \
    curl \
    git \
    gpg-agent \
    locales \
    software-properties-common \
  && add-apt-repository universe \
  && rm -rf /var/lib/apt/lists/* \
  && locale-gen en_US en_US.UTF-8 \
  && update-locale LC_ALL=en_US.UTF-8 LANG-en_US.UTF-8

# Add the ROS1 sources and key
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list' \
  && curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add -

# Add the ROS2 sources and key
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg \
  && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null

# Install ROS 1 and 2 and install the ROS dependencies
RUN apt update \
  && apt upgrade -y \
  && apt install -y \
      python3-colcon-common-extensions \
      python3-rosdep \
      ros-noetic-ros-base \
      ros-foxy-ros-base \
  && source /opt/ros/noetic/setup.bash \
  && source /opt/ros/foxy/setup.bash \
  && rosdep init \
  && rosdep update \
  && rosdep install --from-paths /opt/ros1_bridge/src -iry

# Install the ROS message types required for the bridge
RUN apt install -y \
  ros-noetic-actionlib-msgs \
  ros-foxy-actionlib-msgs \
  ros-noetic-std-msgs \
  ros-foxy-std-msgs \
  ros-noetic-sensor-msgs \
  ros-foxy-sensor-msgs \
  ros-noetic-control-msgs \
  ros-foxy-control-msgs \
  ros-noetic-rosgraph-msgs \
  ros-foxy-rosgraph-msgs \
  ros-noetic-trajectory-msgs \
  ros-foxy-trajectory-msgs \
  ros-noetic-tf2-msgs \
  ros-foxy-tf2-msgs \
  ros-noetic-geometry-msgs \
  ros-foxy-geometry-msgs

# Build the bridge
RUN cd /opt/ros1_bridge \
  && . /opt/ros/foxy/setup.sh \
  && . /opt/ros/noetic/setup.sh \
  && colcon build --symlink-install

# Copy the script and set the entry point
COPY --chmod=755 docker/launch_rosbridge.sh /opt/launch_rosbridge.sh
ENTRYPOINT ["/opt/launch_rosbridge.sh"]
