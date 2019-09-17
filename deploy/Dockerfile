FROM ros:kinetic-perception

ARG graphic

RUN mkdir -p /tmp/runtime-root
ENV XDG_RUNTIME_DIR /tmp/runtime-root
ENV DEBIAN_FRONTEND noninteractive
#########################################################################
# Installing Necessary packages
RUN apt-get update && \
    apt-get install -y software-properties-common \
        python-software-properties \
        debconf-utils apt-utils \
        wget curl \
        binutils \
        python-dev python-pip \
        build-essential autoconf automake && \
    rm -rf /var/lib/apt/lists/*

# Installing tmux
RUN apt-get update && \
    apt-get install -y \
        tmux vim && \
    rm -rf /var/lib/apt/lists/*

#########################################################################
# Build osmesa for software rendering
RUN if [ "$graphic" = "osmesa" ] ; then ( \
        wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key | apt-key add - && \
        apt-add-repository "deb http://apt.llvm.org/xenial/ llvm-toolchain-xenial main" && \
        apt-get update && \
        apt-get install -y clang-5.0 lld-5.0 && \
        rm -rf /var/lib/apt/lists/* && \
        ln -s /usr/bin/llvm-config-5.0 /usr/bin/llvm-config && \
        ln -s /usr/bin/clang-5.0 /usr/bin/clang && \
        ln -s /usr/bin/clang++-5.0 /usr/bin/clang++ && \
        ln -s /usr/bin/clang-cpp-5.0 /usr/bin/clang-cpp \
    ); fi

RUN if [ "$graphic" = "osmesa" ] ; then ( \
        apt-get update && \
        apt-get install -y \
            libtool-bin \
            libx11-dev \
            libxext-dev \
            x11proto-core-dev \
            x11proto-gl-dev \
            libglew-dev \
            freeglut3-dev \
            bison flex \
            pkg-config zlib1g-dev && \
        rm -rf /var/lib/apt/lists/* \
    ); fi

RUN if [ "$graphic" = "osmesa" ] ; then ( \
        cd /tmp && \
        wget https://mesa.freedesktop.org/archive/mesa-18.2.4.tar.xz && \
        tar xf mesa-18.2.4.tar.xz && \
        mkdir mesa-18.2.4/build && \
        cd mesa-18.2.4/build && \
        ../configure --disable-dri \
                    --disable-egl \
                    --disable-gbm \
                    --with-gallium-drivers=swrast,swr \
                    --with-platforms=x11 \
                    --prefix=/usr/local/ \
                    --enable-gallium-osmesa \
                    --disable-xvmc --disable-vdpau --disable-va \
                    --enable-llvm \
                    --with-swr-archs=avx && \
        make -j `numprocs` && make install \
    ); fi

# Setup our environment variables.
RUN if [ "$graphic" = "osmesa" ] ; then ( \
        echo "export XVFB_WHD='1920x1080x24'" >> /root/.bashrc && \
        # echo "export DISPLAY=':99'" >> /root/.bashrc && \
        echo "export LIBGL_ALWAYS_SOFTWARE='1'" >> /root/.bashrc && \
        echo "export GALLIUM_DRIVER='softpipe'" >> /root/.bashrc \
    ); fi

#########################################################################
########## INSTALLING GENERIC ROBOTIC DEVELOPMENT TOOLS HERE ############
#########################################################################
### Colcon
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list' && \
    apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654 && \
    apt-get update && \
    apt-get install -y \
        python3-colcon-common-extensions && \
    rm -rf /var/lib/apt/lists/*
# source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash

RUN apt-get update && \
    apt-get install -y \
        # python-catkin-tools \ ## This causes mysterious linking problems with libstdc++.so (2019 Jan 23)
        # Installing rosbash
        ros-${ROS_DISTRO}-rosbash \
        # Installing RViz
        ros-${ROS_DISTRO}-rviz \
        # Installing rqt
        ros-${ROS_DISTRO}-rqt \
        ros-${ROS_DISTRO}-rqt-common-plugins \
        # Pretty common ROS packages
        ros-${ROS_DISTRO}-joint-state-publisher \
        ros-${ROS_DISTRO}-robot-state-publisher && \
    rm -rf /var/lib/apt/lists/*
#########################################################################

# RUN sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list' && \
#     wget http://packages.osrfoundation.org/gazebo.key -O - | apt-key add - && \
#     apt-get update && \
#     apt-get install -y \
#         ros-${ROS_DISTRO}-gazebo-ros-pkgs \
#         ros-${ROS_DISTRO}-gazebo-ros-control \
#         ros-${ROS_DISTRO}-gazebo-* && \
#     rm -rf /var/lib/apt/lists/*

#########################################################################
# Ref: http://wiki.ros.org/turtlesim
RUN apt-get update && \
    apt-get install ros-${ROS_DISTRO}-turtlesim && \
    rm -rf /var/lib/apt/lists/*

# Upgrade all packages. This will solve the issue for X programs.
RUN apt-get update && \
    apt-get upgrade -y && \
    rm -rf /var/lib/apt/lists/*


RUN echo "source /workspace/devel/setup.zsh --extend" >> /root/.bashrc
RUN echo "chmod -R a+rwx /workspace" >> /root/.bashrc

WORKDIR /workspace
