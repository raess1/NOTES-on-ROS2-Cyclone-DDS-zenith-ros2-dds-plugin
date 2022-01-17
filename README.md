# notes-fmyslef

#ROS 2 Cyclone DDS zenith-ros2-dds plugin

First install ROS2 foxy from steps below
https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Binary.html

Create a Workspace
https://docs.ros.org/en/foxy/Tutorials/Workspace/Creating-A-Workspace.html


#Install the  zenith-ros2-dds plugin
https://github.com/eclipse-zenoh/zenoh-plugin-dds

``cd ~/ros_ws/src/``

``git clone https://github.com/eclipse-zenoh/zenoh-plugin-dds.git``

``cd ~/ros_ws``

``rosdep install -q --from-paths src --ignore-src -y``

``colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release``

``source setup.bash``


#HOST1

``ROS_DOMAIN_ID=1 ros2 run turtlesim turtlesim_node``

``ros2 run zenoh_bridge_dds zenoh_bridge_dds -d 1``

#HOST2

``ROS_DOMAIN_ID=2 ros2 run turtlesim turtle_teleop_key``

``ros2 run zenoh_bridge_dds zenoh_bridge_dds -d 2``




#hosts, avoiding UDP multicast communication

#HOST1

``ROS_DOMAIN_ID=1 ros2 run turtlesim turtlesim_node``

``ros2 run zenoh_bridge_dds zenoh_bridge_dds -d 1``

#HOST2

``ROS_DOMAIN_ID=2 ros2 run turtlesim turtle_teleop_key``

``ros2 run zenoh_bridge_dds zenoh_bridge_dds -d 2 -e tcp/<host-1-ip>:7447``  - where ``<host-1-ip>`` is the IP of host 1


#HOST1 communication outside network. Open port ``7447`` on your host.    


``export CYCLONEDDS_URI=file://path/ddscyclonedds.xml``


#You can also explicitly activate multicast on loopback on Unix systems doing:

``sudo route add -net 224.0.0.0 netmask 240.0.0.0 dev lo``

``sudo ifconfig lo multicast``

#On k3lso run

``zenoh-bridge-dds --no-multicast-scouting -l udp/0.0.0.0:7447``

#In the base station

``zenoh-bridge-dds --no-multicast-scouting -l udp/0.0.0.0:7447 -e udp/IP:7447``



#Open port ubu

``sudo ufw enable``

``sudo ufw allow 7447``

check status ``sudo ufw status verbose``

