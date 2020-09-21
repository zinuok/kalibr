original link: [kalibr](https://github.com/ethz-asl/kalibr)
<br>
***
## install & run for my desktop(Ubuntu 18.04, ROS Melodic)
### install 
+ from [here](https://blog.csdn.net/weixin_43715197/article/details/106730297)
```
$ sudo apt-get install -y python-setuptools python-rosinstall ipython libeigen3-dev libboost-all-dev doxygen libopencv-dev
$ sudo apt-get install -y ros-melodic-vision-opencv ros-melodic-image-transport-plugins ros-melodic-cmake-modules 
$ sudo apt-get install -y software-properties-common libpoco-dev python-matplotlib python-scipy python-git python-pip ipython 
$ sudo apt-get install -y libtbb-dev libblas-dev liblapack-dev python-catkin-tools libv4l-dev
$ sudo pip install python-igraph --upgrade

$ cd ~/catkin_ws/src && git clone https://github.com/ethz-asl/Kalibr.git
$ cd .. && catkin build -DCMAKE_BUILDTYPE=Release -j3
$ source ~/catkin_ws/devel/setup.bash
```

### run
+ step 1) camera calibraion **pinhole-radtan for D435i**
```
$ kalibr_calibrate_cameras --models pinhole-radtan pinhole-radtan --topics /camera/infra1/image_rect_raw /camera/infra2/image_rect_raw --bag [ROS bag file] --target aprilgrid_6x6.yaml
```
+ step 2) verify result (0.1~0.2 px for a good calibration)
```
$ kalibr_camera_validator --chain chain.yaml --target aprilgrid_6x6.yaml
```
+ step 3) camera-imu calibration
```
$ kalibr_calibrate_imu_camera --cam chain.yaml --target aprilgrid_6x6.yaml --imu imu0.yaml --bag [ROS bag file]
```
***

## Trouble shooting
+ Exception in thread block: [aslam::Exception] /home/zinuok/catkin_ws/src/kalibr/aslam_nonparametric_estimation/aslam_splines/src/BSplineExpressions.cpp:447: toTransformationMatrixImplementation() assert(_bufferTmin <= _time.toScalar() < _bufferTmax) failed [1.60032e+09 <= 1.60032e+09 < 1.60032e+09]: Spline Coefficient Buffer Exceeded. Set larger buffer margins!
<br>
=> change timeoffset-padding value to **0.1** at line 95 in kalibr/blob/master/aslam_offline_calibration/kalibr/python/kalibr_calibrate_imu_camera <br>
from [here](https://github.com/ethz-asl/kalibr/issues/41) and [here](https://github.com/ethz-asl/kalibr/blob/master/aslam_offline_calibration/kalibr/python/kalibr_calibrate_imu_camera#L171)
