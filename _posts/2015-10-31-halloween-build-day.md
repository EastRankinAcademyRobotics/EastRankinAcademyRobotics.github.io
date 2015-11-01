---
layout: post
title:  "Halloween Build Day"
date:   2015-10-31 22:45:51
header-img: "haunted_hous1.jpg"
---
Today the build team met at the workshop and constructed the base robot frame with two motor controllers, each controlling two motors (one for each wheel). After they finished the code team started the process of trying to control the robot.

Both phones already had the FTC software installed: the FTC Driver Station app on one, and the FTC Robot Controller app on the other. (Note that the manual says that each phone should one have _one_ app each; both apps should not be installed on the phones.)

# Setting up the Driver Station and Controller

First we connected the driver station phone to the LogiTech F130 USB game controller via the hub and the On The Go (or OTG) cable (note that the manual specifies that the button on the back of the controller should be set to _X_ to have the controller run in XBox emulation mode). We then launched the driver station app, only to see no evidence that the game controller was connected to the phone. __After Googling around we discovered that after connecting the controller to the phone you have to press ```Start + A``` to register the controller with the app as User 1, or ```Start + B``` to register the controller as User 2.__ After doing this the driver station app showed the controller as connected above the appropriate user name in the top right, and was outlined in green when the buttons on the controller were pressed. We later discovered this is in the manual, but its buried and you are likely to read over it if you aren't looking for it.

# Setting up the Robot Controller

Next, we connected the other phone to the robot using the OTG cable connected to the control module on board the robot, and followed the instructions in the manual to map the on board hardware to names that would be used later in the tele-op module. We had to try a few options to figure out what would work, but this is where we ended up:

- Motor Controller 1 (named mc_1)
  - motorleft_1
  - motorleft_2
- Motor Controller 2 (named mc_2)
  - motorright_1
  - motorright_2

Since the robot doesn't have a front or back right now, we arbitrarily picked one side as the left and the other as right.

Then we followed the instructions in the manual and set up WiFi Direct between the driver station and robot controller phones; this pairing went without incident.

# The First Tele-Op Experience: A Good Failure

Next we selected one of the tele-op modes on the driver station app and tried to initialize it. The app failed to initialize, reporting that it failed to find ```servo 1``` -- this was a good sign. We knew from inspecting the code in the tele-op mode we selected (```K9TankDrive```) that it expected a robot with two motors, a claw, and an arm. The arm is controlled by a servo. Since the driver station app was able to report that the expected hardware was not found on the robot, we knew we had a good pairing between the phones and that we were able to communicate with the robot.

# Writing our own Tele-Op Mode

Catherine had written a basic (motors) tele-op mode, called ```TestTeleOp``` while we were waiting for the build team to finish. This mode was based on modifying ```K9TankDrive```, removing the code to control the arm and claw and adding code to control the left two motors as a pair with the left stick, and the right two motors as another pair with the right stick (moving the sticks "up" and "down" results in changing the directions of the motors, thus the robot drives like a tank).

We tried to load that mode onto the robot controller phone. We followed the manual's instructions, but we observed two errors that would not resolve. First, Android Studio was complaining that the SDK needed to be configured, even though we could not determine that any components were missing (we did extensive Googling and determined that we had the pieces of API 19 and 21 that were expected, including the ARM images). The specific error we saw after attempting a build was

> ```Error: Module 'FTCRobotController': platform 'Google Inc.:Google APIs:19' not found```

Second, even though the Android Device Manager showed the phone connected, the Android monitor pane in Studio reported that no device was connected, even after we verified that the phone was properly connected and in debug mode.

## Hello World

After failing to get anything to work or to find a solution on the web, we used Studio to write a basic "hello world" app as discussed in the manual (this is the app that you get if you start Android Studio, select create new project from the startup wizard, and just load the default code that is automatically created). This app, which doesn't use the FTC SDK, worked just fine, loading to the phone with no problem and opening with the expected "hello world" window. Exploring the interface to compare how the SDK settings differed between the application that worked and the FTC app that didn't (```Option ;``` in Studio brings up the project structure pane and allows you to explore the SDK settings for the FtcRobotController module) revealed a difference between the compile SDK version and the version of the build tools that the two apps were using. The FTC app was set to use API 19 and version 21.1.1 of the build tools; the working "hello world" app used the newer versions. The figure below shows the window in the interface with the default settings for the FTC app.

{% include figure.html img="/assets/2015-10-31/default.png" sm="/assets/2015-10-31/default_sm.png" caption="The default SDK settings for ftc_app; click for larger image." %}

Even though the FTC app settings agreed with what the manual specified, we tried setting the FTC app to match the working "hello world" app, suspecting that there might be a difference in behavior caused by the fact that we are using a Mac and the manual is written for Windows. Indeed, when we changed the settings as shown in the figure below, the phone suddenly appeared in the Android monitor pane and the interface gave us the option of loading the app to our phone during the build and launch process. Success!

{% include figure.html img="/assets/2015-10-31/correct.png" sm="/assets/2015-10-31/correct_sm.png" caption="The SDK settings for ftc_app that eventually worked; click for larger image." %}

Note that we think all of this is related to the unresolved Google API 19 error that we saw and ignored when we were setting up the SDK, as [discussed here](http://eastrankinacademyrobotics.github.io/2015/10/16/installing-the-sdk/).

## Loading the Tele-Op Mode

Now we were able to load Catherine's tele-op mode onto the robot controller phone. The first time we did this the Android Studio interface warned us that there was already an app on the phone of the same name, and that proceeding would result in that app being uninstalled. This is covered in the manual. It turns out that the app that contains the tele-op modes and all the code we will write for this project is the robot controller app itself, so our builds replace that app but, happily, the configuration file we created that maps the robot components to names is preserved (we also noticed that, after replacing the app we downloaded from Google Play with our own app, subsequent builds and installations onto the phone do not ask us if we want to uninstall the app that is already on the phone; it just happens automatically).

After selecting ```TestTeleOp``` mode on the driver station, initiating operation, verfying the WiFi Direct connection was set up, and pressing the play button on the driver station app, we were able to drive the robot using the game controller.

# Summary

This ended the day with a complete working prototype of the software build and robot control process, from writing a tele-op mode to compiling and loading it on the phone, pairing the phones, and driving the robot with the game controller.

The manual turned out to be an important and fairly complete resource today. Everything we struggled with was covered in the manual (although not always in a way that we understood before we knew the solution to the problem) with the important exception of having to set the build tools and the SDK version to a version much newer than the one the manual says we need to use. This may be a Mac vs. Windows issue, but it cost us a lot of time and frustration figuring out.
