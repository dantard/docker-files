#### STEPS ####
#en /etc/docker/daemon.json poner

#{
#  "data-root": "/home/danilo/dockers"
#}


#docker build -t ros1-rviz:v1 /path/to/your/dockerfile/directory

### RUN: run the container (the --rm makes it temporary and removed when CTRL+D-ed)

# NO 
#sudo docker run -v /home/danilo/:/felix -it --rm --env="DISPLAY" --env="QT_X11_NO_MITSHM=1" --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" --name instance1 ros1-rviz:v1

# YES
#sudo docker run -v /home/danilo/:/felix -it --rm --env="DISPLAY" --env="QT_X11_NO_MITSHM=1" --env=XAUTHORITY=/tmp/.Xauthority -e XAUTHORITY=${XAUTH} --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" --device=/dev/dri:/dev/dri --name instance1 ros1-rviz:v1

#Also, note that users in the container (other than root) need access to the 'video' group for Mesa DRI devices.

### ATTACHING: using the following will attach a console (bash) to the running container
### sudo docker exec -it instance1 bash


# Use the official ROS image as the base
FROM ros:noetic

# The build get stuck when asking for keyboard country without this
ENV DEBIAN_FRONTEND=noninteractive

# Install rviz and some utilities
RUN apt-get update && apt-get install -y ros-noetic-rviz terminator iputils-ping net-tools joe libgl1-mesa-glx libgl1-mesa-dri

RUN rm -rf /var/lib/apt/lists/*


# Create a non-root user named "danilo" and make it sudo-capable without password
RUN useradd -m -s /bin/bash danilo && \
    echo "danilo ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Set the user to "danilo"
USER danilo

# Set the working directory
WORKDIR /home/danilo

# Source ROS setup for the non-root user
RUN echo "source /opt/ros/noetic/setup.bash" >> /home/danilo/.bashrc


# Start a bash shell when the container runs
CMD ["/bin/bash"]
