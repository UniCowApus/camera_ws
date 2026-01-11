This is more of a cautionary tale than a readme. I have also not slept in the last 48 hours. Originally I was going to make this an ansible package or even just a bash script but I'm out of time before submission. This is the edge node component of the stereo vision tracker, and was built with the following. 

Hardware
- A raspberry pi 5 or jetson nano (borrowed from Queen's AutoDrive and Queen's Aerospace teams respectively). This will not work on <=pi4, since you need the dual 22way ribbon connectors.
- [Waveshare binocular camera with 2 day shipping](https://www.waveshare.com/imx219-83-stereo-camera.htm?srsltid=AfmBOooJMTAcICeUELW7Q3bKGrryskYOkGS6iUz6ofTSxDRu6jwfLcEx)
- 3D printed camera mount
- Printed [characu board](https://docs.opencv.org/4.x/df/d4a/tutorial_charuco_detection.html] on 8.5x11)

![Epic setup](camera_ws/image.png)


Software
- The pi is running ubuntu 24.04LTS. In order to use the camera module with the board mounted connectors, rpi foundation has forked libcamera, and includes with with raspbian, but raspbian doesn't support ros2 natively, so you cannot use raspbian. Libcamera (now rpicam) is not shipped or built for ubuntu with support for boardmounted connectors and cannot be installed by package manager. You must compile this library yourself, so I've included it as a submodule.
- I also publish my images using two instances of a camera_ros node. I have forked it and included it as as submodule as well with a stereo camera launch file, and stereo camera intrinsics/extrinsics config files as well, which are loaded and published as camera_info messages. When you install rosdeps for this, it will try and use your system libcamera, or overwrite the forked one that you just compiled, but this can be avoided with -skip-keys libcamera.
- Additionally, you will need to enable overlays for the camera drivers by adding:

dtoverlay=imx219,cam0

dtoverlay=imx219,cam1 

to the [all] section of /boot/firmware/config.txt, and reboot.

After this is set up, you should be able to just launch using my provided launch file in camera_ros. I am sorry these instructions are so poor, but I am out of time (:



