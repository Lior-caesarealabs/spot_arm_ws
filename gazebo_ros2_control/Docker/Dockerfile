FROM osrf/ros:humble-desktop

# Make sure everything is up to date before building from source
RUN apt-get update \
  && apt-get upgrade -y \
  && apt-get clean

RUN apt-get update && apt-get install -q -y --no-install-recommends \
    dirmngr \
    gnupg2 \
    lsb-release \
    python3-colcon-ros \
    && apt-get clean

RUN mkdir -p /home/ros2_ws/src \
    && cd /home/ros2_ws/src/ \
    && git clone -b humble https://github.com/ros-controls/gazebo_ros2_control \
    && rosdep fix-permissions && rosdep update \
    && rosdep install --from-paths ./ -i -y --rosdistro humble \
      --ignore-src

RUN cd /home/ros2_ws/ \
  && . /opt/ros/humble/setup.sh \
  && colcon build --merge-install

COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

CMD ros2 launch gazebo_ros2_control_demos cart_example_position.launch.py
