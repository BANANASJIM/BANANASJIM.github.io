---
layout: post
title: 'Physics-Based Animation'
---

## Rigid Body Simulation

This project is part of the **Games103** course, an introduction to physics-based animation. The course focuses on various foundational methods for simulating physical phenomena in animations and games. For this specific project implements a **Rigid Body Simulation** using **Leapfrog Integration** for position and orientation updates, along with collision detection and response.

You can view the demo of this simulation in the video below:

[![Rigid Body Simulation Demo](https://img.youtube.com/vi/4S1pmGpo0ro/0.jpg)](https://youtu.be/4S1pmGpo0ro)


## Cloth Simulation
This project implements a **Cloth Simulation** using both **Implicit Cloth Solver** and **Position-Based Dynamics (PBD)** techniques.

1. Implicit Cloth Solver
- **Initial Setup**: Applied damping to vertex velocities and calculated initial position guesses.
- **Gradient Calculation**: Computed the gradient of the objective function using vertex positions and forces (spring and gravity).
- **Vertex Update**: Used a simplified Newton’s method with a diagonal Hessian matrix to iteratively update vertex positions.
- **Sphere Collision**: Detected collisions with a sphere and applied impulse-based responses to colliding vertices.

1. Position-Based Dynamics (PBD)
- **Initial Setup**: Dampened velocity, applied gravity, and updated vertex positions.
- **Strain Limiting**: Implemented Jacobi-based position updates to preserve edge lengths and prevent stretching.
- **Sphere Collision**: Handled vertex collisions with a sphere using the same method as the implicit solver.

You can view the demo of this simulation in the video below:
[![Cloth Simulation Demo](https://img.youtube.com/vi/w0aH0qq6UKs/0.jpg)](https://youtu.be/w0aH0qq6UKs)
S

## Water Wave Simulation
This project implements a **Water Wave Simulation** using a simplified shallow wave model and two-way coupling techniques.

1. **Shallow Wave Model**
   - **Ripple Simulation**: Simulated water surface ripples by updating vertex heights based on neighboring vertices and damping factors.
   - **User Interaction**: Allowed users to add random water drops, adjusting vertex heights to maintain constant water volume.

2. **Two-Way Coupling**
   - **Rigid Body Dynamics**: Applied computed pressures as forces and torques to blocks, enabling mutual influence between blocks and water.
   - **Collision Handling**: Managed interactions between blocks and water surface, ensuring realistic responses to movements and collisions.


You can view the demo of this simulation in the video below:
[![Water Wave Simulation Demo](https://img.youtube.com/vi/udTKbeNV2fE/0.jpg)](https://youtu.be/udTKbeNV2fE)
## About Games103
**[Games103 course](https://games-cn.org/games103/)** is a foundational course that introduces students to a range of physics-based animation. It covers the simulation of physical phenomena such as rigid body dynamics, soft body deformation, fluid dynamics, and cloth simulation. 

