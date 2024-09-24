# rosbot-autonomy

## Simulation

> [!IMPORTANT]
> To run `Gazebo` or `Webots` Simulators you have to use computer with NVIDIA GPU and the [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) installed.

If you don't have a physical ROSbot 2R / 2 PRO you can run this project in a simulation.

![Gazebo](https://github-readme-figures.s3.eu-central-1.amazonaws.com/rosbot/rosbot-autonomy/gazebo-rviz.png)

### Gazebo

To start Gazebo simulator run:

```bash
just start-gazebo-sim
```

This will start the simulation and SLAM.

### Webots

To start Webots simulator run:

```bash
just start-webots-sim
```
