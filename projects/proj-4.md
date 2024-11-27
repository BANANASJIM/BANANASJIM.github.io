---
layout: post
title: 'Character Animation'
---

In this project, I implemented a basic version of motion matching to achieve more natural character animation transitions.

Principle of Motion Matching

Motion matching is a technique that selects the most appropriate animation frame from a pre-recorded dataset by comparing the current state with a set of target states. Instead of defining every possible animation transition manually, motion matching lets the character dynamically choose the best animation based on desired movement goals such as direction, velocity, and pose similarity.

Core Steps:

Data Collection: A wide variety of movement animations (e.g., walk, run, jump) are captured and stored in an animation database.

Search Matching: At runtime, the character's current movement state is continuously compared against the animation dataset to find the best possible frame that matches the intended action.

Animation Playback: The best-matched animation frames are used, resulting in smooth and responsive character transitions.

You can view the demo of this simulation in the video below:
[![Motion Matching](https://img.youtube.com/vi/FGjM8A0G008/0.jpg)](https://youtu.be/FGjM8A0G008)

In addition to motion matching, a Proportional-Derivative (PD) controller was implemented to ensure that the character maintains balance during dynamic movements. The PD controller works by applying corrective forces to the character joints to minimize errors between the target and actual positions, effectively preventing the character from falling.