# Use the official Ubuntu 16.04 image as the base image
FROM ubuntu:16.04

# Install required packages
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

# Install Docker
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" && \
    apt-get update && \
    apt-get install -y docker-ce

# Install ROS
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list' && \
    curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add - && \
    apt-get update && \
    apt-get install -y \
        ros-kinetic-desktop-full \
        python-rosdep \
        python-rosinstall \
        python-rosinstall-generator \
        python-wstool \
        build-essential

# Set up ROS environment
RUN echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc

# Set up Catkin tools
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list' && \
    wget http://packages.ros.org/ros.key -O - | apt-key add - && \
    apt-get update && \
    apt-get install -y python3-catkin-tools

# Install Gazebo
RUN sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list' && \
    wget http://packages.osrfoundation.org/gazebo.key -O - | apt-key add - && \
    apt-get update && \
    apt-get upgrade -y

# Create a Catkin workspace and set it up
RUN mkdir -p ~/catkin_ws/src
WORKDIR /root/catkin_ws
RUN catkin init

# Clone the SD-TwizyModel repository
RUN cd ~/catkin_ws/src && \
    git clone https://github.com/streetdrone-home/SD-TwizyModel.git

# Update Python version to 3.6
RUN apt-get install -y software-properties-common && \
    add-apt-repository -y ppa:jblgf0/python && \
    apt-get update && \
    apt-get install -y python3.6 && \
    rm /usr/bin/python3 && \
    ln -s /usr/bin/python3.6 /usr/bin/python3

# Set Python 3.6 as the default
RUN echo "alias python3='/usr/bin/python3.6'" >> ~/.bash_aliases && \
    echo "alias python3='/usr/bin/python3.6'" >> ~/.bashrc

RUN echo "python3 --version"
RUN echo "source /opt/ros/kinetic/setup.bash"
RUN catkin config  --extend /opt/ros/kinetic

# Build the Catkin workspace
RUN rosdep init && \
    rosdep update && \
    rosdep install --from-paths src/ --ignore-src -r -y && \
    rosdep update && \
    catkin build

RUN echo "xhost +local:docker" 

# Set up the entry point with display forwarding and launching the application
CMD ["/bin/bash", "-c", "source ~/catkin_ws/devel/setup.bash && roslaunch sd_robot sd_twizy_empty.launch"]
