# Differential Drive Gazebo Simulator for ROS2 Humble
This repository contains the code and configuration files to launch a GoBilda differential drive robot simulation in Gazebo with ROS2 Humble. The simulation runs inside a Docker container for a consistent and reproducible environment.

## Pre-requisites
- Docker installed and configured on your system

## Setup Guide & Quick Start Guide
Follow these steps to get the simulator running

### 1. Clone this repository
Start by cloning this repository to your local machine:
```bash
git clone https://github.com/ambulantelab/gobilda_sim.git
cd gobilda_sim
```

### 2. Edit the Makefile to properly pull the correct docker image from DockerHub
- The correct Docker image must be selected for your computer's architecture (x86 for Intel/AMD PCs, ARM for Apple Silicon Macs).
- Find and modify the IMAGE_NAME line:
- For x86 PCs (Intel/AMD): Use x86.
- For ARM PCs (Apple M1/M2): Use arm.


### 3. Create the Docker Network
Run the following command to create a shared network that allows the simulator and visualization tools to communicate. You only need to do this once.
```bash
docker network create ros
```

### 4. Start the Web Visualizer (ncVNC)
This command starts a web-based visualizer. You will use your browser to view the Gazebo simulation and RViz.
```bash
make novnc
```
After running this, open your web browser and go to: http://localhost:8080/vnc.html. You should see a desktop environment.

### 5. Run the simulation image and mount your local copy of this repository
Open a new terminal window in the gobilda_sim directory. Run the following command to start the simulator container and mount your local code into it.
```bash
make bash NAME=sim_test
```
This will drop you into a bash shell inside the running Docker container. Your terminal prompt will change to something like root@container_id:/workspace#.

If you get errors about not having the image locally, you can run the following command to manually pull the image from DockerHub (remember your platform):
```bash
docker pull ambulantelab/cpe416:lab4-arm
```

### 6. Build and Source the Workspace (Inside the Container)
From the bash shell inside your container, run:
```bash
colcon build --symlink-install
source install/setup.bash
```

### 7. Launch the Simulation (Inside the Container)
Finally, launch the main simulation launch file:
```bash
ros2 launch ros_gz_example_bringup diff_drive.launch.py
```

You should now see the Gazebo simulation window open in the noVNC browser tab you opened in Step 4, and the robot will be spawned.
Finally, you should press play in the Gazebo simulator to start the simulation. After, you should see the topics and robot model in RViz.
