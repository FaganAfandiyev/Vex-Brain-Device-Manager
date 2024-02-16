# TerriBull Device Manager

## Overview
The TerriBull Device Manager is a comprehensive system designed for managing various types of VEX devices. It provides a framework for initializing and controlling devices such as motors, IMUs, and other peripherals using a unified interface. This system is built on top of the PROS library for VEX Robotics, leveraging its functionalities for device communication and control. More specific documentation related to using the library will be coming soon.

## Features
- **Device Support**: Supports a wide range of devices including motors, IMUs, distance sensors, vision sensors, and more.
- **Dynamic Initialization**: Devices can be dynamically initialized with specific configurations such as gear sets and brake modes for motors.
- **Velocity Control**: Allows setting motor velocities with fine-tuned control, considering the gear set configuration.
- **Extensible Design**: Designed to be easily extendable for additional device types and functionalities.

## Requirements
- PROS library for VEX Robotics
- nanopb
- arm-none-eabi compiler Toolset for Linux or WSL

## Installation/Compilation
1. Ensure the PROS library is installed and properly configured in your development environment.
2. Clone this repository into your project's directory.
3. Include the `TerriBull.hpp` and the `Devices` directory in your project's include paths.

### Creating protobuf Definitions:
1. Ensure `nanopb` is installed in the project.
2. cd into project

```bash
python python nanopb/generator/nanopb_generator.py --out=. ./include/protos/vex.proto
```

- You should now have generated the `.pb.h` and `.pb.c` files.

- To finish this process, find the `vex.pb.h` file and modify the top of the file to look like this:
```c
/* Automatically generated nanopb header */
/* Generated by nanopb-0.4.9-dev */

#ifndef PB_TERRIBULLDEVICES_INCLUDE_PROTOS_VEX_PB_H_INCLUDED
#define PB_TERRIBULLDEVICES_INCLUDE_PROTOS_VEX_PB_H_INCLUDED
#include <pb.h>
#include <pb_encode.h>
#include <pb_decode.h>
#include <pb_common.h>
```

### Creating the obj Files
- Ensure you have `arm-none-eabi` compiler for Cortex

```bash
mkdir ./obj/
arm-none-eabi-g++ -c -o ./obj/vex.pb.o ./include/protos/vex.pb.c -I./nanopb/ -I. 
arm-none-eabi-g++ -c -o ./obj/pb_encode.o ./nanopb/pb_encode.c -I./nanopb/ -I. 
arm-none-eabi-g++ -c -o ./obj/pb_decode.o ./nanopb/pb_decode.c -I./nanopb/ -I. 
arm-none-eabi-g++ -c -o ./obj/pb_common.o ./nanopb/pb_common.c -I./nanopb/ -I.
```
- You should now have the obj files linked in the obj/ directory.

Run:

```bash
arm-none-eabi-ar rcs firmware/libvex.a obj/vex.pb.o obj/pb_encode.o obj/pb_decode.o obj/pb_common.o
```

### Building with Pros
- Ensure you have Pros CLI installed on your computer.
- An error will occur if 
Run:
```bash
pros build --verbose
```

## Usage

### Initializing a Motor
```cpp
DeviceManager deviceManager;
motor_initialize_callback_data motorData = {port, GEAR_SET, BREAK_MODE};
deviceManager.motor_device_initialize(motorData);
```

### Setting Motor Velocity
```cpp
motor_set_velocity_callback_data velocityData = {port, velocity};
deviceManager.motor_device_set_velocity(velocityData);
```

### Adding and Managing Devices
Devices are managed through the `DeviceManager`, which allows adding, retrieving, and configuring devices through a unified interface.

## Contributors
- Jonathan Koch: `jonathan.koch@caemilusa.com`
- 

## License
TODO