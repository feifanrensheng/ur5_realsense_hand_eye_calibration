# UR5 REALSENSE CALIBRATION


### Requires

Make sure the following environment:

* ROS kinetic
* MoveIt 
* librealsense & realsense-ros

### Prepare
1. Build catkin workspace

```
mkdir -p ~/catkin_ws/src
cd  ~/catkin_ws
catkin_make
```

2. Install dependence packages:

```
$ sudo apt-get install ros-kinetic-visp
$ sudo apt-get install ros-kinetic-industrial-msgs
$ sudo apt-get install ros-kinetic-ros-control ros-kinetic-ros-controllers
```


3. Download the calibration lib
```
$ cd ~/catkin_ws/src
$ git clone https://github.com/portgasray/ur5_realsense_calibarion.git
```
or
```
$ git clone https://github.com/portgasray/ur5_realsense_calibarion.git
$ cd  ur5_realsense_calibarion
$ git submodule foreach --recursive git submodule init
$ git submodule foreach --recursive git submodule update
```
4. Perform catkin make
```
$ catkin_make
```

:star: Before excuting roslaunch command, you should to preform belowing cmd, **Always remember it !**

```
cd ~/catkin_ws
source ./devel/setup.bash
```
or a convenient way（if you have at least two workspaces, not recommended)
```
$ echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
$ source ~/.bashrc
```
### Excution

* launch communicaiton with real ur5 robot using IP
```
$ cd ~/catkin_ws
$ roslaunch ur_modern_driver ur5_bringup.launch limited:=true robot_ip:=192.168.50.101
```

* launch moveit 
```
$ cd ~/catkin_ws
$ roslaunch ur5_moveit_config ur5_moveit_planning_execution.launch limited:=true
```
* launch calibration program
```
$ cd ~/catkin_ws
$ roslaunch easy_handeye ur5_d435i_test.launch
```

After that, you will see **three** GUI following(make sure your librealsense was installed at single path and the version accord with environment):

1. ![Image text](./images/automatic_movement.png)

2. ![Image text](./images/easy_handeye.png)

3. ![Image text](./images/rviz.png)

In the GUI window 'rqt_easy_handeye.perspective -rqt': 

Plugins ->  Visualization -> Image View
(you may maximize this window to see the options on the left-top)

![Image text](./images/rqt_image_viewer.jpg)

select "/aruco_tracker/result" in listbox

Let the  broad in the center of your camera screen

![Image text](./images/center_of_screen.jpg)

See the Calibrator automatic movement 

Begin start, Pleas slow down the speed of your robot

![Image text](./images/automatic_movement.png)

Click Check starting Pose Button

Take (Next Pose) (Plan) (Excute) (Take Sample) as a loop

** Just remember that, robot move a new pose than Take  Sample **

** If board move out of sight, do not press (Take Sample) button, it may occur bug **

** Plz take more than 8 sample, then press (Compute) button to get 7-element-matrix **

 Save ! Finish!(The .yaml file will save at ~/.ros/easy_handeye/)

![Image text](./images/loop.png)

You can have a look by publishing your calibration result.

1. Edit launch file and roslaunch it(without close other program window).
 1.1 copy parameters in .yaml to ur5_realsense_handeyecalibration_after2.launch(arg:x,y,z,qx,qy,qz,qw)
 ```
 sudo gedit ~/catkin_ws/src/ur5_realsense_calibarion/easy_handeye/easy_handeye/launch/ur5_realsense_handeyecalibration_after2.launch
 sudo gedit ~/.ros/easy_handeye/ur5_realsense_handeyecalibration_eye_on_base.yaml
 ```
 ![Image text](./images/copy_matrix_to_launch1.png)
 ![Image text](./images/copy_matrix_to_launch2.png)
 
 1.2 now roslaunch the launch file
```
(do not run this command)roslaunch  easy_handeye  publish.launch eye_on_hand:=false namespace_prefix:=ur5_realsense_handeyecalibration
(run this)roslaunch easy_handeye ur5_realsense_handeyecalibration_after2.launch 
```
2.Add TF to see calibration result(in rviz)
 if you find TF flash in rviz
```
sudo gedit ~/catkin_ws/src/ur5_realsense_calibarion/easy_handeye/easy_handeye/launch/calibrate.launch
```
comment out this code patch and re-launch
![Image text](./images/calibrate_launch_comment_out.png)

using easy_handeye publish result

```
roslaunch easy_handeye publish.launch eye_on_hand:=true

```


author: Ning Zhang, Beihang University, China.
email:zy1703239@buaa.edu.cn
