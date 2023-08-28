# Use the official Ubuntu 16.04 image as the base image
FROM mjenz/ros-kinetic-desktop-full

RUN apt-get update && apt-get install -y \
    curl \
    apt-transport-https \
    ca-certificates \
    software-properties-common \
    lsb-core \
    wget \
    git \
    nano \
    x11-apps

RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list' 
RUN wget http://packages.ros.org/ros.key -O - | apt-key add - && \
    apt-get update && \
    apt-get install -y python3-catkin-tools

RUN sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list' 
RUN wget http://packages.osrfoundation.org/gazebo.key -O - | apt-key add - && \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get dist-upgrade -y


RUN mkdir -p ~/catkin_ws/src
WORKDIR /root/catkin_ws
RUN catkin init

RUN cd ~/catkin_ws/src && \
    git clone https://github.com/streetdrone-home/SD-TwizyModel.git

RUN apt-get install -y software-properties-common && \
    add-apt-repository -y ppa:jblgf0/python && \
    apt-get update && \
    apt-get install -y python3.6 && \
    rm /usr/bin/python3 && \
    ln -s /usr/bin/python3.6 /usr/bin/python3

RUN echo "alias python3='/usr/bin/python3.6'" >> ~/.bash_aliases && \
    echo "alias python3='/usr/bin/python3.6'" >> ~/.bashrc
RUN echo "python3 --version"
RUN echo "source /opt/ros/kinetic/setup.bash"
RUN catkin config  --extend /opt/ros/kinetic

RUN rm /etc/ros/rosdep/sources.list.d/20-default.list
RUN rosdep init && \
    rosdep update && \
    rosdep install --from-paths src/ --ignore-src -r -y 
RUN catkin build

RUN apt-get install -y xterm

RUN cd /root/catkin_ws

RUN echo "source /root/catkin_ws/devel/setup.bash" >> ~/.bashrc 


RUN sysctl -w kernel.shmmax=2147483648