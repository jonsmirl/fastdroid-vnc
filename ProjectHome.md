# Fastdroid VNC server for Android #

## The first Android VNC server that really works! ##

Started with original fbvncserver for the iPAQ and Zaurus, incorported
changes by Jim Huang <jserv.tw@gmail.com>, and Steve Guo (letsgoustc).

New features by Danke Xie (danke.xie@gmail.com)
  * Added input device search to support different devices
  * Added kernel vnckbd driver to allow full-keyboard input on 12-key hw
  * Supports Android framebuffer double buffering
  * Performance enhancement and fixes of GCC warnings in libvncserver-0.9.7

## SUMMARY OF FEATURES ##
  * Android VNC server through TCP/IP connection (default port: 5901)
  * Support Android framebuffer double buffering
  * Support keyboard/mouse input interactivity
  * Optional kernel keyboard driver to support remote full keyboard
  * Optimized for fast response time and reduced unnecessary wake-ups

## ANDROID QUICK INSTALL ##

  * Download the binary here: http://code.google.com/p/fastdroid-vnc/downloads/list
  * Use adb to push to android
```
$ adb push fastdroid-vnc /data/
```
  * Run on the phone or emulator
```
$ adb shell chmod 755 /data/fastdroid-vnc
$ adb shell /data/fastdroid-vnc
```

  * Emulator setup
```
telnet localhost 5554        // telnet to emulator
 redir add tcp:5900:5901     // map host port 5900 to emulated device port 5901
 exit                        // exit emulator shell

vncviewer localhost          // connect to vnc server 
```

  * Phone setup. If the VNC server is running on a real device, one may want to setup wifi   or ethernet tethering on android. For tethering, please refer to information about Android cdc-ecm or RNDIS to do so.

![http://fastdroid-vnc.googlecode.com/files/screenshot-emulator.png](http://fastdroid-vnc.googlecode.com/files/screenshot-emulator.png)