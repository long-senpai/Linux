## Install GSTREAM LIB IN RASPI 4

```
$ sudo apt-get install libx264-dev libjpeg-dev

$ sudo apt-get install libgstreamer1.0-dev \
     libgstreamer-plugins-base1.0-dev \
     libgstreamer-plugins-bad1.0-dev \
     gstreamer1.0-plugins-ugly \
     gstreamer1.0-tools \
     gstreamer1.0-gl \
     gstreamer1.0-gtk3 \ 
     gstreamer1.0-pulseaudio

$ sudo apt-get install gstreamer1.0-qt5

```

## Test command


- In Bullseye, the only working camera source now is libcamerasrc, so that if your camera is USB camera, you have to install the ubuntu os to RPi instead of the Raspians OS. 
```
For usb camera
    $ gst-launch-1.0 v4l2src device="/dev/video0" name=e ! 'video/x-raw, width=640, height=480' ! videoconvert ! 'video/x-raw, width=640, height=480, format=(string)YUY2' !  udpsink host=127.0.0.1 port=5000 
! xvimagesink
Note: 
- device="/dev/video0" is the address of the usb camera 

```
## UDP streamming

### In RASPI side
- The data will be direct forward to the PC IP address/Port

- replace this line by your IP -  "host=127.0.0.1 " // Port as you want
```
$ gst-launch-1.0 -v v4l2src device=/dev/video0 num-buffers=-1 ! video/x-raw, width=640, height=480, framerate=30/1 ! videoconvert ! jpegenc ! rtpjpegpay ! udpsink host=127.0.0.1 port=5200
```
### In the reciever side 
```
$ gst-launch-1.0 -v udpsrc port=5200 ! application/x-rtp, media=video, clock-rate=90000, payload=96 ! rtpjpegdepay ! jpegdec ! videoconvert ! autovideosink
```
