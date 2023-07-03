# STEEReoCAM streaming in ISAAC ROS Platform

## Prerequisites
	
  - Jetpack version 5.1.1
  - Jetson AGX Orin/ Xavier (other devkits are not supported )

## BSP Package Installation
	
1. Download the BSP package from [here](https://ftp.e-consystems.com/nextcloud/index.php/s/7FMMsHfgrexTnBd)

2. Copy the release package to the home directory of the Jetson development kit and run the following commands to extract the release package to obtain the         binaries:
		
        tar -xaf e-CAM20_Stereo_CUMI2311_ISSAC_JETSON_L4T35.3.1_03-JULY-2023_R01.tar.gz
        cd e-CAM20_Stereo_CUMI2311_ISSAC_JETSON_L4T35.3.1_03-JULY-2023_R01
	
3. Run the following commands to install the binaries:

        sudo -E ./install_binaries.sh

4. The above script will reboot the Jetson development kit automatically after installing the binaries successfully.

## Enumerating STEEReoCAM as an ISAAC ROS Argus Camera

1. Set up your development environment by following the steps given [here](https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/blob/main/docs/dev-env-setup.md)

2. Clone the ISAAC ROS dependent repositories under `~/workspaces/isaac_ros-dev/src`
  
   Note : (Keep repository as `/ssd/workspaces/isaac_ros-dev/src` if you have setup your docker environment in a SSD)

        cd ~/workspaces/isaac_ros-dev/src 
        
        git clone https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common
   
        git clone https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_nitros
   
        git clone https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_image_pipeline
   
        git clone https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_argus_camera

4. Launch the docker container using the run_dev.sh script in terminal 1:

        cd ~/workspaces/isaac_ros-dev/src/isaac_ros_common && \
        ./scripts/run_dev.sh 
	
5. Inside the container, build and source the workspace:

        cd /workspaces/isaac_ros-dev && \
        colcon build --symlink-install && \
        source install/setup.bash
  
6. To stream in ISAAC ROS platform, we have two nodes: the publisher node (terminal 1) which launches the argus camera stream and the                  
   subscriber node (terminal 2) which receives the data from the publisher.

7. In terminal 1, run the following launch file to stream the argus camera:

        ros2 launch isaac_ros_argus_camera isaac_ros_argus_camera_mono.launch.py
	
8. Open another terminal and launch the docker container:

        cd ~/workspaces/isaac_ros-dev/src/isaac_ros_common && \
        ./scripts/run_dev.sh 
	
9. Source the workspace in terminal 2: 
		
        source install/setup.bash

10. In terminal 2, run the following commands to view the live stream of argus camera:

        ros2 run image_view image_view --ros-args -r image:=/left/image_raw
