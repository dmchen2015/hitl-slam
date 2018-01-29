# Human-in-the-Loop SLAM

This repository contains a C++ implementation of Human-in-the-Loop SLAM, for use in post-hoc metric map repair and correction. A ROS 
wrapper is provided, which handles communication between GUI and backend. The source code is designed to be readable and extensible, and 
can be easily modified to fit the needs of a particular project.

- Author: [Samer Nashed](TODO)

## Publication

If you use this software in an academic work or find it relevant to your research, kindly cite:

```
@inproceedings{ghosh2017joint,
  doi = { 10.1109/IROS.2017.8202271 },
  url = { https://www.joydeepb.com/Publications/jpp.pdf },
  pages = { 1026--1031 },
  organization = { IEEE },
  year = { 2017 },
  booktitle = { Intelligent Robots and Systems (IROS), 2017 IEEE/RSJ International Conference on },
  author = { Sourish Ghosh and Joydeep Biswas },
  title = { Joint Perception And Planning For Efficient Obstacle Avoidance Using Stereo Vision },
}
```

Link to paper: [https://www.joydeepb.com/Publications/jpp.pdf](https://www.joydeepb.com/Publications/jpp.pdf)

## Dependencies

- A C++ compiler (*e.g.*, [GCC](http://gcc.gnu.org/))
- [cmake](http://www.cmake.org/cmake/resources/software.html)
- [popt](http://freecode.com/projects/popt)
- [libconfig](http://www.hyperrealm.com/libconfig/libconfig.html)
- [Boost](http://www.boost.org/)
- [OpenCV](https://github.com/opencv/opencv)
- [OpenMP](http://www.openmp.org/)

Use the following command to install dependencies:

```bash
$ sudo apt-get install g++ cmake libpopt-dev libconfig-dev libboost-all-dev libopencv-dev python-opencv gcc-multilib
```

For compiling and running the ROS wrapper, install [ROS Indigo](http://wiki.ros.org/indigo/Installation/Ubuntu).

## Compiling

### 1. Building JPP libraries and binaries

Clone the repository:

```bash
$ git clone https://github.com/umass-amrl/jpp
```

The script `build.sh` compiles the JPP library:

```bash
$ cd jpp
$ chmod +x build.sh
$ ./build.sh
```

### 2. Building JPP ROS

For compiling the ROS wrapper, `rosbuild` is used. Add the path of the ROS wrapper to `ROS_PACKAGE_PATH` and put the following line in your `.bashrc` file. 
Replace `PATH` by the actual path where you have cloned the repository:

```bash
$ export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:/PATH/jpp/ROS
```

Execute the `build_ros.sh` script:

```bash
$ chmod +x build_ros.sh
$ ./build_ros.sh
```

## Running JPP on AMRL and KITTI Datasets

### 1. Download Datasets

The complete example data (AMRL and KITTI) along with calibration files can be found 
[here](https://greyhound.cs.umass.edu/owncloud/index.php/s/3g9AwCSkGi6LznK).

### 2. Running JPP

After compilation, the `jpp` binary file is store inside the `bin/` folder. For processing a single pair of stereo images, use:

```bash
$ ./bin/jpp -l [path/to/left/image] -r [path/to/right/image] -c [path/to/stereo/calibration/file] -j [path/to/jpp/config/file] -o [output_mode]
```

For processing multiple stereo pairs stored in a directory, use:

```bash
$ ./bin/jpp -n [number of pairs] -d [path/to/directory] -c [path/to/stereo/calibration/file] -j [path/to/jpp/config/file] -o [output_mode]
```

**Note:** stereo image pairs inside the directory must be named like this: `left1.jpg`, `left2.jpg`, ... , `right1.jpg`, `right2.jpg`, ...

For the example datasets, calibration files are stored in the `calibration/` folder and JPP configurations are stored in the `cfg/` folder. JPP operates on 
3 output modes (set by the `-o` flag) as of now: `astar`, `rrt`, and `debug` mode. Set the flag `-v 1` for generating visualizations.
