# rosbot-autonomy

A step-by-step guide for the ROSbot 2R/PRO to map an unknown environment and navigate autonomously within it from RViz. Works over the Internet thanks to Husarnet VPN

## Connecting ROSbot and laptop over VPN

Ensure that both ROSbot 2R and your laptop linked to the same Husarnet VPN network. If they are not follow these steps:

1. Setup a free account at [app.husarnet.com](https://app.husarnet.com/), create a new Husarnet network, click the **[Add element]** button and copy the code from the **Join Code** tab.
2. Connect your laptop to the [Husarnet network](https://husarnet.com/docs). If you are Ubuntu user, just run:

   ```bash
   curl https://install.husarnet.com/install.sh | sudo bash
   ```

   and connect to the Husarnet network with:

   ```bash
   sudo husarnet join <paste-join-code-here>
   ```

3. Connect your ROSbot to the Husarnet network. Husarnet is already pre-installed so just run:

   ```bash
   sudo husarnet join <paste-join-code-here> rosbot2r
   ```

## Repository Setup

This repository contains the Docker Compose setup for both PC and ROSbot 2, 2R and 2 PRO. You can clone it to both PC and ROSbot 2, 2R and 2 PRO, or use the `./sync_with_rosbot.sh` script to clone it to your PC and keep it synchronized with the robot

```bash
git clone -b foxglove https://github.com/husarion/rosbot-autonomy
cd rosbot-autonomy
./sync_with_rosbot.sh rosbot2r # Or clone the same repo on your rosbot
```

## Verifying User Configuration

To ensure proper user configuration, review the content of the `.env` file and select the appropriate configuration (the default options should be suitable).

- **`LIDAR_BAUDRATE`** - depend on mounted LiDAR
- **`MECANUM`** - wheel type
- **`SLAM`** - choose between mapping and localization modes
- **`SAVE_MAP_PERIOD`** - period of time for autosave map (set `0` to disable)
- **`CONTROLLER`** - choose the navigation controller type

## I. Running on a Physical Robot

### ROSbot 2, 2R and 2 PRO

Run Docker images defined in `compose.yaml` inside `rosbot-autonomy` on ROSbot:

#### Pulling the latest version

```bash
docker compose pull
```

#### Flashing the ROSbot's Firmware

To flash the Micro-ROS based firmware for STM32F4 microcontroller responsible for low-level functionalities of ROSbot 2, 2R and 2 PRO, execute in the ROSbot's shell:

```bash
./flash_firmware.sh
```

#### Running autonomy

```bash
docker compose up
```

### PC

Open the **Google Chrome** browser on your laptop and navigate to:

http://rosbot2r:8080/ui

#### Result

![autonomy-result](.docs/autonomy-result.gif)

> [!NOTE]
> To instruct the robot to autonomously explore new areas and create a map (in "slam" mode) of **[Goal Pose]** in Foxglove. Please note that whenever you disable `SLAM`, you must disable the containers with the `docker compose down` command. When `SLAM` is off, you can indicate the robot's current position by changing the published message from **/goal_pose** to **/initialpose** topic.

---

## II. Simulation

> [!IMPORTANT]
> The `compose.sim.gazebo.yaml` and `compose.sim.webots.yaml` files use NVIDIA Container Runtime. Make sure you have NVIDIA GPU and the [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) installed.

### Gazebo

1. Start the containers in a new terminal:

   ```bash
   xhost +local:docker && \
   docker compose -f compose.sim.gazebo.yaml up
   ```

2. Then open the **Google Chrome** browser on your laptop and navigate to:

   http://localhost:8080/ui

### Webots

1. Start the containers in a new terminal:

   ```bash
   xhost +local:docker && \
   docker compose -f compose.sim.webots.yaml up
   ```

2. Then open the **Google Chrome** browser on your laptop and navigate to:

   http://localhost:8080/ui

## Developer info

### Add pre-commit

[pre-commit configuration](.pre-commit-config.yaml) checks file formats before contributing. Usage:

```bash
# install pre-commit
pip install pre-commit

# initialize pre-commit workspace
pre-commit install

# manually run tests
pre-commit run -a
```
