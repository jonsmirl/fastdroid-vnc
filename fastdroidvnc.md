# Fastdroid VNC server for Android #

## The first Android VNC server that really works! ##

Started with original fbvncserver for the iPAQ and Zaurus.

Modified by Jim Huang <jserv.tw@gmail.com>
  * Simplified and sizing down
  * Performance tweaks

Modified by Steve Guo (letsgoustc)
  * Added keyboard/pointer input

**Modified by Danke Xie for Android (danke.xie@gmail.com)**
  * Added input device search to support different Android devices
  * Added kernel vnckbd driver to allow full-keyboard input on 12-key hw
  * Supports Android framebuffer double buffering
  * Performance enhancement and fixes of GCC warnings in libvncserver-0.9.7

## Android quick install ##

  * Download the binary here: http://code.google.com/p/fastdroid-vnc/downloads/list
  * Use adb to push to android
```
$ adb push fastdroid-vnc /data/
```
  * Run on the phone
```
$ adb shell
# cd data
# chmod 777 fastdroid-vnc
# ./fastdroid-vnc
```

## How to get source ##
The latest source can be downloaded with subversion from the googlecode server.
```
svn checkout http://fastdroid-vnc.googlecode.com/svn/trunk/ fastdroid-vnc-read-only
```

Source tarball releases can be downloaded on the download page:
http://code.google.com/p/fastdroid-vnc/downloads/list

## How to contribute ##
Please post comments and issues on the "issues" page of this project. If you would like
to submit a change, welcome to join the project.

## Android build ##

Copy the directory fastdroid-vnc to any Android build directory, run
a build command from Android root
```
  $ make -j4 fastdroid-vnc
```

Push the binary to the device
```
  $ adb push system/bin/fastdroid-vnc /data/
```

Modify file permission and run
```
  $ adb shell
  # cd /data
  # chmod 777 fastdroid-vnc
  # ./fastdroid-vnc
```

## Linux build ##

This Android VNC server may also work on other Linux-based systems. An example
Makefile is included, which should be able to build the binary with the make command.

## Input support ##

This version of the fbvncserver can forward input events from the VNC client
to Android (based on Steve Guo's work). The way it works is converting remote
VNC inputs to Linux input events and send to devices `/dev/input/event<N>`.

Each input device has a specific `<N>` value. The server can automatically detect
the `<N>` value based on the name string of the device.  It can enumerate the input
devices under `/device/input/event<N>`, and select the device matching the
keywords in fbvncserver.c. For example, they keywords are

```
static const char *KBD_PATTERNS[] = {"VNC", "key", "qwerty", NULL};
static const char *TOUCH_PATTERNS[] = {"touch", "qwerty", NULL};
```

If an input device contains the strings in KBD\_PATTERNS, it will be used to send
the keyboard events. The same is true fo rthe touchpad devices matching
the strings in TOUCH\_PATTERNS. If multiple devices match, the one matching the
left-most pattern is used.

If a matched device is not found, then default input devices will be used. The
user can also specify the keyboard and touchpad devices by the command line:

```
-k <keyboard-device-path>
-t <touchpad-device-path>
```

Information about the input devices and its name strings can be found in
`/proc/bus/input/devices`.

## Full keyboard input driver (optional) ##

If the device has a numeric keypad, the input device cannot be used by the
VNC server to send alphabetic inputs to the Android framework. In this case,
one can install the "vnckbd" driver included in this release. The instructions
and source can be found in the kernel subdirectory.

## VNC performance enhancement ##

This VNC server is based on libvncserver 0.9.7. Several optimizations
are made upon the server to improve the response time to VNC clients, and
reduced background activity when there is no active clients.

It also supports the double buffering mechanism used by Android. This can
avoid frame misses found in previous Android framebuffer VNC servers.

  * Fixed VNC frame misses under Android double-buffering framework
  * Removed 100ms wake-ups in connection waiting
  * Sped up message queue draining to allow faster responses
  * Fixed misc libvncserver warnings