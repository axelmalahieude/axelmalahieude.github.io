---
layout: page
permalink: /sw-projects/aviata
---
<link rel="stylesheet" href="/assets/css/style.css">
<h1 id="top">AVIATA: Autonomous Vehicle Infinite Air-Time Apparatus</h1>

This year-long project was completed during my time at UCLA as a part of the UAS@UCLA club organization, which focuses on unmanned aerial systems. As I have graduated, I am no longer working on this research project, but the club is continuing to work on it. This project was sponsored by NASA as part of a grant received through the <a href="https://nari.arc.nasa.gov/usrc" target="_blank">University Student Research Challenge</a> program.

Project homepage: <a href="https://uasatucla.org/en/aviata" target="_blank">https://uasatucla.org/en/aviata</a>

Github repository: <a href="https://github.com/uas-at-ucla/aviata" target="_blank">https://github.com/uas-at-ucla/aviata</a>

My role in this research project involved developing a system to enable autonomous docking of drones to an airborne structure. To accomplish this, drones were fitted with a Raspberry Pi and a camera, and computer vision targets were placed on each docking location. With my team, I wrote and tested code that enabled the drone to detect the target and adjust its flight path in order to approach the target. This was done using a PID controller, which was fine-tuned enough to allow for centimeter-level precision during the final stages of docking.

We also developed a simulation using Gazebo, a flight controller simulation software, so we could test our code before putting it on a real drone.

We started in Python, but switched to C++ after discovering that we would get a significant performance improvement when running on a Raspberry Pi. We also used the <a href="https://mavsdk.mavlink.io/develop/en/cpp/" target="_blank">MAVSDK</a> library for sending commands to the drone, <a href="https://april.eecs.umich.edu/software/apriltag" target="_blank">AprilTag</a> for target detection, <a href="https://docs.opencv.org/4.5.1/d1/dfb/intro.html" target="_blank">OpenCV</a> for image manipulations and reading from the on-board camera, and <a href="http://abyz.me.uk/rpi/pigpio/" target="_blank">pigpio</a> for using the GPIO pins on the Raspberry Pi.

Here is a video of the drone achieving 2-stage docking, in which it uses a larger, centrally-located target to position itself before homing in on the smaller target used for docking.

<div class="image-container">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/u5uN2gBfQ1c" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>
<br>

Here is a video of the drone once I had tuned the PID controller enough to dock inside the docking cone, which in the future will be placed on the AVIATA system for airborne docking.

<div class="image-container">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/Biwi7Z0oJk8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>
<br>

Finally, here is a video in which I explain the AVIATA project. This video was created for NASA's 2021 Transformative Aeronautics Concepts Program (TACP) showcase.

<div class="image-container">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/c2nyjCBQSm8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>
<br>