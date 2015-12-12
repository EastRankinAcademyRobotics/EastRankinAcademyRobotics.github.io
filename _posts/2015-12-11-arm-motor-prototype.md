---
layout: post
title:  "Arm Motor Prototype"
date:   2015-12-11 17:45:51
header-img: ""
---
Over the past weeks the build team has been finishing the arm prototype; the prototype has two motors that drive the elevation of the arm and one motor that extends the length of the arm. Catherine had written code to control the robot drive plus arm. At the very end of study hall we connected everything up and the robot was unresponsive to the controller; the rest of the evening was spent debugging the system.

During this debug session we found several things.

# Motor Controllers and Single Legacy Controller Ports

During the initial examination of the wiring we found that motor controller 1 had its power polarity reversed; we fixed this with no change in behavior. All other wires were traced and found to be connected correctly. The next debugging step was to comment out the new sections of code and verify that the drive functions worked in the same way they did before we added the new code to control the arms; commenting out the new code did not change the robot's behavior (it remained unresponsive to the game controller). At this point we suspected something in the hardware configuration rather than the software, and disconnected the two new motor controllers that had been daisy chained to the original two controllers, which were themselves daisy chained from a single port (S0) on the legacy controller.

With only the two daisy chained drive controllers connected to port S0, the robot drive became responsive to the game controllers, but only on one side. At this point I directly connected the two drive motor controllers to the legacy controller, one in S0 and one in S1. The robot returned to its previous state of operation and could be driven.

Now I directly connected all four motor controllers to ports S0-3 on the legacy controller, and verified that all of the code Catherine had originally written with me worked as expected.

## What was the Root Cause?

Upon reflection I believe the root problem was that all four motor controllers were daisy chained together onto a single port on the legacy controller, S0. But the hardware map in the robot controller configuration file expected each motor controller to be connected to its own port. Although I fixed the problem by directly connecting motor controllers to their own ports, I believe it would also work to change the configuration file to reflect 6 motors on a single legacy controller port (S0), provided there isn't a limitation on how many devices a single port can control.

# Direction

After the robot returned to controllable operation, we spend time verifying that the code was written such that the motors in each part of the robot ran in the natural sense (i.e., stick up corresponds to forward, etc.). The robot functions are currently mapped as follows:

|  Gamepad | Control Element  | Function  |
|---|---|---|
| 1  |  Left Stick | 2 left drive motors  |
| 1  | Right Stick | 2 right drive motors  |
| 2  | Left Stick | 2 arm elevation motors  |
| 2  | Right Stick | 1 arm extension motors  |

By sheer luck we got all the motors right except for the arm extension motor, which expected on stick down (opposite the expected direction). We fixed this by changing the sign on the data pulled in from the game controller.

# Prototype Performance

The video linked in below shows the performance of the arm prototype. Although we fiddled with code that translates the deflection of the gamepad stick into motor power for the arm elevation motors, we have not been successful in being able to control the arm elevation. Power has to be applied in short bursts to prevent the arm slamming into one side or the other and, when the arm is positioned as desired the arm falls down to a resting position when power is removed.

<!-- Start EasyHtml5Video.com BODY section -->
<style type="text/css">.easyhtml5video .eh5v_script{display:none}</style>
<div class="easyhtml5video" style="position:relative;max-width:360px;"><video controls="controls" poster="/assets/2015-12-11/eh5v.files/html5video/IMG_0963_480.jpg" style="width:100%" title="IMG_0963_480">
<source src="/assets/2015-12-11/eh5v.files/html5video/IMG_0963_480.m4v" type="video/mp4" />
</video><div class="eh5v_script"><a href="http://easyhtml5video.com">html5 embed video</a> by EasyHtml5Video.com v3.5</div></div>
<script src="/assets/2015-12-11/eh5v.files/html5video/html5ext.js" type="text/javascript"></script>
<!-- End EasyHtml5Video.com BODY section -->

As the video makes clear, we may need to re-evaluate the design of the arm.
