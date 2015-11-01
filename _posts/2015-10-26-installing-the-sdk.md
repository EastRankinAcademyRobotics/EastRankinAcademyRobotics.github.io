---
layout: post
title:  "Installing the SDK"
date:   2015-10-26 12:04
header-img: ""
---
After setting the phones up we installed [Android Studio](http://developer.android.com/sdk/index.html?gclid=Cj0KEQjwnrexBRDNmZzNkf7c4c4BEiQALnlxhcCDaf8a_J0w0BxSlF-E7gwb6c2YkBwud2lyrYOR0mIaAmpa8P8HAQ) and the FirstTech Challenge Software Development Kit (SDK) [from Github](https://github.com/ftctechnh/ftc_app), following the instructions in [this video](https://www.youtube.com/watch?v=UtKbbi31PWc&feature=youtu.be).

After installing Android Studio and opening a new project for the first time we did encounter the error he references around minute 2:58 which is caused not having the right components installed for this phone in Studio. After updating the components that Studio indicated needed updating (primarily API 23 stuff), we followed Varun's suggestions and download the Android 5.0.1 (API 21) SDK and Google API components using the SDK Manager. However, this left us with an error related to API 19

> Error: Cause: Failed to find target Google Inc.: Google APIs: 19: /Users/eraprinter/Library/Android/sdk

So we downloaded those components as well, and then got this error:

> Error: failed to find Build Tools revision 21.1.2 +
> Install Build Tools 21.1.2 and sync project.

The last line was hyperlinked, so we clicked it and installed the requested component. This did not eliminate the API 19 error from the error messages window, however, even when closing and reopening the project and restarting Android Studio. However we have tried building the project and it builds fine with no errors and the API 19 error has not reappeared, so we believe that Studio was behaving in an unexpected way or that we simply did not understand what we were seeing.

Next we went through the rest of the tutorial, creating TestTeleOp and walking through the video steps. Around minute 10:08 Varun references ```public void start()``` as the name of the routine that is executed when the op mode is first enabled; in the version of the FTC SDK we downloaded, this routine has been renamed to ```public void init()``` (this change is confirmed on page 94 of _v0_95_ of the FTC Training Manual, located in _ftc_app_master/doc/tutorial/FTCTraining_Manual.pdf_; this manual is very detailed and helpful; all team members should at least skim it so that they know where to look when problems arise --  it is online [here](https://github.com/EastRankinAcademyRobotics/ftc_app/blob/master/doc/tutorial/FTCTraining_Manual.pdf)).

Starting at 21:54 Varun has some nice tips about how to make the phone more usable for robot control (renaming the device for WiFi Direct, disabling apps on the phone to make the phone cleaner to use, etc.)

After this he goes through downloading the TeleOp mode to the robot test vehicle, connecting the driver station and the controllers, and demonstrates the robot moving in response to commands. At this point he's demonstrated everything that needs to be done to drive the robot.
