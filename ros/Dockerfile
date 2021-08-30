FROM balenalib/aarch64-ubuntu:focal
ARG DEBIAN_FRONTEND=noninteractive

# Update Packages
RUN apt-get update
RUN apt-get install -y apt-utils
RUN apt-get install locales && locale-gen en_US en_US.UTF-8 && update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8 && export LANG=en_US.UTF-8

# Install ROS Package Keys
RUN apt-get install -y curl gnupg2 apt-transport-https gnupg ca-certificates lsb-release
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key  -o /usr/share/keyrings/ros-archive-keyring.gpg
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

# Update package repo for ROS
RUN apt-get update

# Install ROS
RUN apt-get update && apt-get install -y ros-foxy-desktop ros-foxy-demo-nodes-cpp ros-foxy-gazebo-ros2-control-demos xfce4 xinit xserver-xorg xserver-xorg-video-fbdev xinit pciutils xfonts-100dpi xfonts-75dpi xfonts-scalable 
RUN apt-get install -y python3-argcomplete wget unzip nano build-essential xterm gedit firefox ros-foxy-turtlebot3 ros-foxy-turtlebot3-gazebo ros-foxy-turtlebot3-msgs ros-foxy-dynamixel-sdk ros-foxy-turtlebot3

## FOR DEVELOPMENT
# Install SSH :: ref https://dev.to/s1ntaxe770r/how-to-setup-ssh-within-a-docker-container-i5i
RUN apt update && apt install  openssh-server sudo -y
# RUN useradd -rm -d /home/ubuntu -s /bin/bash -g root -G sudo -u 1000 test 
# RUN  echo 'test:test' | chpasswd

# RUN  echo 'root:toor' | chpasswd
RUN service ssh start
EXPOSE 22

COPY id_ed25519.pub /root/.ssh/authorized_keys
COPY sshd_config /etc/ssh/sshd_config

# Configure Enviroment for ROS
RUN echo "source /opt/ros/foxy/setup.bash" >> ~/.bashrc
COPY start.sh /start.sh
CMD ["/bin/sh", "start.sh"]
