---
layout: page
permalink: /sw-projects/music-visualizer
---
<link rel="stylesheet" href="/assets/css/style.css">
<h1 id="top">3D Music Visualizer</h1>
This was a team project for Computer Science 174A: Computer Graphics, a course taken in 2020 at UCLA. I had one teammate for this project, and together we decided for our capstone project to create a 3D animation that reacts in real time to music. To do this, we used WebGL and JavaScript to process a song as it plays and use it as input to an animation that moves our objects around the scene.

This visualizer shows our understanding of 3D graphics and linear algebra, as well as a few advanced topics. I personally implemented the ability to render shadows in the animation, while my teammate added realistic simulated physics.

Unfortunately, because this was a class project, I did not make the GitHub repository public. However, the following video shows our animation in action with voiceover explaining how and what we did to implement it.

<div class="image-container">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/l-An80fVh20" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>
<br>

Details about included features:

**Audio Analysis**: The audio track was able to be loaded and played using Javascript's builtin WebAudio API. An Analyzer node was placed to extract frequency data, which was computed every frame while the audio was playing. The FFT size was 128, meaning there were 128 ranges of frequencies extracted with amplitude.

**Light Tunnel**: A cylindrical tunnel shape was created, and each vertex was translated and colored in the shaders based on the frequency data for that frame. Previous frames' frequency data was also stored and displayed to give the tunnel "moving" effect. The tunnel is symmetrical about the y-axis, and the vertex translation is proportional to the amplitude of the frequency corresponding to that vertex.

**Ball Physics**: When the song increases in intensity, a group of balls are instantiated with random velocity vectors. The motion of the ball is defined by this initial velocity vector and a global acceleration constant (to simulate gravity). The laws of motion are used to move the ball through the tunnel. Ball collisions with the deformed tunnel shape are also calculated, and effectively "bounces" off the tunnel with a damped velocity.

**Cube**: A series of linear transformations was used to create 6 planes, each of 16 cubes. Each of the 16 cubes is scaled in height based on the amplitude of a specific frequency. These 6 planes were then rotated to create a cube shape. Finally, the cubes were drawn a second time but with white lines instead of triangles to create the outline. The cube also rotates with respect to the volume, which Anchal determined by averaging the amplitude of all frequencies at each moment in time. As the volume increases, the cube rotates faster and grows in scale.

**Shadows**: As described in the video, a 2-pass rendering technique was used. First, we draw objects that can cast shadows from the light's point of view. This means we use the light transform instead of the camera transform in our matrix multiplication. We also use an orthographic projection for this matrix. For each fragment, we determine its distance from the light source, and use that as the fragment's color value. This frame is then drawn to a texture map using a framebuffer so that we don't see it on screen. During the 2nd pass, we draw objects that receive shadows (i.e., objects onto which shadows are casted upon) using the texture map we created. For each fragment, we determine its distance relative to the light using the same technique as above. We compare this depth to the value stored in the texture map. If the fragment is farther from the light than what is stored in the texture map (i.e., we cannot see this fragment from the light's point of view), then the fragment must be in shadow. We can then decrease the intensity of the color for that fragment to create our shadow.
