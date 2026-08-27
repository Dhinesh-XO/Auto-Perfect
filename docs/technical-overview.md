# Technical overview

## Robot interface

The AgileX Piper-X is a six-axis robotic arm with an actuated gripper. Development used the manufacturer's public Python SDK as the supported interface to the robot's CAN communication layer.

At a high level, the robot interface was responsible for:

- connecting to the configured CAN interface;
- enabling the arm and gripper under controlled conditions;
- reading joint and gripper state;
- issuing bounded joint and gripper commands;
- reporting connection or motion errors to the surrounding system.

No SDK source is vendored in this repository. The official upstream project is linked in [references.md](references.md).

## ROS 2 integration

ROS 2 connected the robot interface with visualization and higher-level task execution. Robot state could be represented through standard joint-state and transform workflows, while task and status messages provided separation between requests and physical execution.

RViz supported inspection of the robot model, joint configuration, transforms, and perception-related markers. Rerun was used during development for richer inspection of spatial and execution data. Internal recordings are not redistributed here.

## Guarded execution

Physical manipulation requires more than forwarding a target directly to the robot. The engineering workflow considered:

1. validating the requested task;
2. checking command ranges and execution state;
3. generating or selecting a motion sequence;
4. executing with conservative limits;
5. monitoring feedback and status;
6. stopping or recovering when execution could not continue safely.

This repository describes those responsibilities at a general level and does not expose the employer's exact thresholds, configuration, or implementation.

## Agent-assisted task execution

The experimental autonomy direction investigated the use of structured high-level requests to initiate sequences of basic manipulation actions. This sat above the robot-control and safety layers: higher-level decisions still needed to pass through controlled motion execution before reaching hardware.

The research direction was influenced by ENPIRE's broader idea of using experience and evaluation to improve robotic policies. This public case study does not claim an exact reproduction, equivalent results, or implementation of every ENPIRE component.
