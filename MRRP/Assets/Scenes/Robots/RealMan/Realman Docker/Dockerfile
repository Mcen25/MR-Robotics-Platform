# Use ROS Noetic base image
FROM ros:noetic-ros-base

# Ensure system packages are up to date and install dependencies
RUN apt-get update && apt-get install -y \
    vim \
    iputils-ping \
    net-tools \
    python3-pip \
    dos2unix \
    ros-noetic-robot-state-publisher \
    ros-noetic-moveit \
    ros-noetic-rosbridge-suite \
    ros-noetic-joy \
    ros-noetic-ros-control \
    ros-noetic-ros-controllers \
    ros-noetic-tf2-web-republisher \
 && rm -rf /var/lib/apt/lists/*

# Install Python dependencies for Noetic (Python 3)
RUN pip3 install --no-cache-dir rospkg jsonpickle

# Set workspace environment variable
ENV ROS_WORKSPACE=/catkin_ws

# Copy project source files into the workspace
COPY ./ROS/src/moveit_msgs $ROS_WORKSPACE/src/moveit_msgs
COPY ./ROS/src/niryo_moveit $ROS_WORKSPACE/src/niryo_moveit
COPY ./ROS/src/niryo_one_ros $ROS_WORKSPACE/src/niryo_one_ros
COPY ./ROS/src/niryo_one_urdf $ROS_WORKSPACE/src/niryo_one_urdf
COPY ./ROS/src/ros_tcp_endpoint $ROS_WORKSPACE/src/ros_tcp_endpoint

# Copy setup and tutorial scripts
COPY ./docker/set-up-workspace /setup.sh
COPY docker/tutorial /

# Ensure files are in Unix format
RUN /bin/bash -c "find $ROS_WORKSPACE -type f -print0 | xargs -0 dos2unix"

# Convert and run setup
RUN dos2unix /tutorial /setup.sh && \
    chmod +x /setup.sh && \
    /setup.sh && \
    rm /setup.sh

# Set working directory
WORKDIR $ROS_WORKSPACE

# Make sure Python scripts are executable
RUN chmod +x src/niryo_moveit/scripts/*.py

# Entry point for the container
ENTRYPOINT ["/tutorial"]
