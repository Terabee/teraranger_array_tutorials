# Setup before launching SLAM and AMCL algorithm

First create a workspace:
```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin_make
```
For using the SLAM and the AMCL with teraranger tower you need some packages.
Enter in your workspace and git clone them like this:

- **If you have setup your ssh key**

```
cd ~/catkin_ws/src
git clone git@github.com:Terabee/teraranger_array.git 
git clone git@github.com:Terabee/teraranger_array_converter.git
git clone git@github.com:Terabee/teraranger_array_tutorials.git
git clone git@github.com:Terabee/teraranger_description.git
cd ~/catkin_ws
catkin_make
source devel/setup.bash
```
- **If you prefer to use https**

```
cd ~/catkin_ws/src
git clone https://github.com/Terabee/teraranger_array.git
git clone https://github.com/Terabee/teraranger_array_converter.git
git clone https://github.com/Terabee/teraranger_description.git
git clone https://github.com/Terabee/teraranger_array_tutorials.git
cd ~/catkin_ws
catkin_make
source devel/setup.bash
```

# SLAM with Kobuki

First install the following packages:
- SLAM: `sudo apt-get install ros-<your_distro>-slam-gmapping`
- Turtlebot: `sudo apt-get install ros-<your_distro>-turtlebot`
- Turtlebot Gazebo: `sudo apt-get install ros-<your_distro>-turtlebot-gazebo`

Source the bashrc file: `source ~/.bashrc`

Launch a roscore: `roscore`

**Verify that the tower and kobuki are connected to your computer and turn ON.**

After that you have to launch two different files in two different terminals:

*!WARNING! When opening a new terminal, don't forget to source the devel !WARNING!* 

- The bring up: 
`roslaunch teraranger_array_tutorials kobuki_bringup.launch`  

Or if you want to simulate on Gazebo:
`roslaunch teraranger_array_tutorials kobuki_gazebo_bringup.launch` 
 
- The driver for the teraranger_one tower:
`roslaunch teraranger_array_tutorials gmapping.launch` 

You can modify the parameters of the gmapping package into the teraranger_array_tutorials package:
- teraranger_array_tutorials/launch/includes/gmapping.launch.xml 

 **Notice that the actual settings should be the best one, find after some tests.**

You can use the kobuki teleop for moving the robot:
`roslaunch turtlebot_teleop keyboard_teleop.launch`

You can watch the map using RVIZ and looking at the map topic. It's easier to understand what happens if you add the robot model and the laser scan topics.

When the map is drawn like you want don't forget to save it
`rosrun map_server map_saver -f ~/catkin_ws/src/teraranger_array_tutorials/maps/<name_of_the_map>`

# AMCL with Kobuki 

Verify you have the map in the right directory: ~/catkin_ws/src/teraranger_array_tutorials/maps

Please modify the map name with the one you want to use for the navigation into the amcl.launch file at this line:
=> arg name="map_file" default="$(find teraranger_array_tutorials)/map/name_of_the_map.yaml

- Launch the bringup in a first terminal:
`roslaunch teraranger_array_tutorials kobuki_bringup.launch`

- Then you can launch the AMCL file into a second one:
`roslaunch teraranger_array_tutorials amcl.launch`

You can modify the parameters inside the file: teraranger_array_tutorials/launch/includes/amcl.launch.xml

### How to use AMCL

Make an initial pose on RVIZ with the arrow on the top (you need to be in "map" fixed frame in RVIZ), place it at the position of your robot on the map with the good direction(front of the arrow represent the front of the robot).

Then you can use Set a Goal with RVIZ, the robot will try to reach the goal you sent him on the map 

