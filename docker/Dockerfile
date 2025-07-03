# === Base Image ===
FROM nvidia/cuda:12.3.1-runtime-ubuntu22.04

# Install ROS 2 on top
RUN apt-get update && apt-get install -y curl gnupg lsb-release

# Add ROS 2 key and repo
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key | apt-key add - && \
    sh -c 'echo "deb http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'


# === Fix tzdata prompt ===
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC
RUN ln -fs /usr/share/zoneinfo/Etc/UTC /etc/localtime && \
    apt-get update && \
    apt-get install -y tzdata && \
    dpkg-reconfigure --frontend noninteractive tzdata


# Install ROS Humble desktop
RUN apt-get update && apt-get install -y ros-humble-desktop-full

# === Install rosdep ===
RUN apt-get update && apt-get install -y python3-rosdep

# === Initialize rosdep ===
RUN rosdep init && rosdep update

ENV DEBIAN_FRONTEND=noninteractive
SHELL ["/bin/bash", "-c"]

# === Basic Dev Tools & Common Libraries ===
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    curl \
    wget \
    vim \
    git \
    python3-pip \
    python3-colcon-common-extensions \
    usbutils \
    cmake \
    ninja-build \
    libgtest-dev \
    libgoogle-glog-dev \
    libboost-dev \
    libeigen3-dev \
    liblua5.3-dev \
    graphviz \
    libgraphviz-dev \
    libcurl4-openssl-dev \
    libsdl2-dev \
    joystick \
    libspdlog-dev \
    libgl1-mesa-glx \
    libxext6 \
    libxrender1 \
    libxtst6 \
    libxi6 \
    libxrandr2 \
    libxinerama1 \
    libxcursor1 \
    && rm -rf /var/lib/apt/lists/*

# === Python deps for Booster SDK ===
RUN pip3 install pybind11 pybind11-stubgen

# === Clone and build Booster SDK ===
WORKDIR /root/Workspace
RUN git clone https://github.com/BoosterRobotics/booster_robotics_sdk.git && \
    cd booster_robotics_sdk && \
    chmod +x install.sh && \
    ./install.sh && \
    mkdir build && cd build && \
    cmake .. -DBUILD_PYTHON_BINDING=on && \
    make && \
    make install

# === Copy your entire project ===
COPY ./ /root/Workspace/booster_sim

# Download all zips in one go to /tmp
WORKDIR /tmp

RUN curl -L -o webots.zip "https://github.com/socrob/booster_sim/releases/download/v1.0/webots.zip" && \
    curl -L -o worlds.zip "https://github.com/socrob/booster_sim/releases/download/v1.0/worlds.zip" && \
    curl -L -o sim_control.zip "https://github.com/socrob/booster_sim/releases/download/v1.0/sim_control.zip" && \
    curl -L -o fastdds-monitor.zip "https://github.com/socrob/booster_sim/releases/download/v1.0/fastdds-monitor.zip"

# Unpack them into the right places
WORKDIR /root/Workspace/booster_sim

RUN unzip /tmp/webots.zip && mv webots /usr/local/webots && \
    unzip /tmp/worlds.zip && \
    unzip /tmp/sim_control.zip && \
    unzip /tmp/fastdds-monitor.zip && \
    rm /tmp/*.zip


# === Set Webots environment variables ===
ENV WEBOTS_HOME=/usr/local/webots
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$WEBOTS_HOME/lib/controller
ENV PATH=$WEBOTS_HOME:$WEBOTS_HOME/bin:$PATH
RUN echo "export PATH=\$WEBOTS_HOME:\$WEBOTS_HOME/bin:\$PATH" >> ~/.bashrc


# === Make sure your runner has execute permissions ===
RUN chmod +x /root/Workspace/booster_sim/sim_control/booster-runner-full-webots-7dof_arms-0.0.3.run && \
    chmod +x /root/Workspace/booster_sim/sim_control/booster-runner-webots-full-0.0.10.run


# === Aliases ===
RUN echo "alias start-webots='cd /root/Workspace/booster_sim/worlds && webots T1_7dofs_arm_on_ground.wbt'" >> ~/.bashrc
RUN echo "alias start-runner='cd /root/Workspace/booster_sim/sim_control && ./booster-runner-full-webots-7dof_arms-0.0.3.run'" >> ~/.bashrc
RUN echo "alias sdk-client='cd /root/Workspace/booster_robotics_sdk/build && ./b1_loco_example_client 127.0.0.1'" >> ~/.bashrc


# === ROS 2 Workspace ===
WORKDIR /root/Workspace/ros2_ws
RUN mkdir -p src

# === Clone booster ROS 2 packages ===
WORKDIR /root/Workspace/ros2_ws/src
RUN git clone https://e.coding.net/generate-gjjleqkqwr/booster-robotics/booster_ros2_interface.git
RUN git clone https://e.coding.net/generate-gjjleqkqwr/booster-robotics/booster_ros2_example.git


# Install ROS dependencies & build
WORKDIR /root/Workspace/ros2_ws
RUN source /opt/ros/humble/setup.bash && \
    rosdep update && \
    rosdep install --from-paths src --ignore-src -r -y && \
    colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release

# Auto-source ROS 2 workspace
RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc && \
    echo "source /root/ros2_ws/install/setup.bash" >> ~/.bashrc

# === FastDDS profile ===
ENV FASTRTPS_DEFAULT_PROFILES_FILE=/root/Workspace/booster_sim/fastdds_profile.xml

# === Helpful ROS2 aliases ===
RUN echo "alias restart-daemon='ros2 daemon stop && ros2 daemon start && ros2 topic list'" >> ~/.bashrc
RUN echo "alias cb='colcon build'" >> ~/.bashrc
RUN echo "alias S='source /opt/ros/humble/setup.bash && source /root/Workspace/ros2_ws/install/setup.bash'" >> ~/.bashrc

# === Default ===
CMD ["bash", "-c", "source /opt/ros/humble/setup.bash && source /root/Workspace/ros2_ws/install/setup.bash && exec bash"]
# CMD ["bash"]



