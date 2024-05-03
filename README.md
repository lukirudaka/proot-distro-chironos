ROOTFS IS ON GOOGLE DRIVE. WILL ADD LINK HERE WHEN FINISHED UPLOADING

This is based on Ubuntu-LTS, and uses Mesa Turnip drivers to provide full hardware acceleration under PRoot-Distro in Termux to KDE Plasma 6, and any other applications.

It was last updated on May 3rd 2024 and is made for ARM64 devices.

WHAT DOESN'T WORK:
- Compositors
     Compositors, upon trying to use the Zink driver, will cause everything to go black.
     Either use a compositor that has 100% compatibility with Vulkan, or use a standalone compositor on software rendering. There is already one configured in this fashion
- Firefox-ESR
Upon trying to use EGL forcibly in Firefox, it makes Firefox instantly crash upon startup. You **can** install Chromium from ppa:xtradeb/apps to use Vulkan on Chromium, at the cost of having Google spy on you.
- Any themes that use the blur effect.
Enabling the blur effect on Picom -- because we can't get any compositor working with hardware acceleration -- will make the usability go down the drain and cause massive lag. Transparency still works just fine with no issues.

WHAT DOES:
Pretty much everything. KDE Plasma 6 under Mesa Zink is VERY responsive, and leaves plenty of room to do whatever you want to do on the device I tested with, which was a Samsung Galaxy Tab S6 Lite (Model SM-P613).

WHAT IT COMES WITH:
- KATE
- KDE Plasma 6
- picom (for Software-rendered compositing, remedies broken compositors without adding too much load to the CPU)
- onboard (Virtual keyboard for those not using hackers keyboard like me)
- Firefox-ESR (Just to get you started. You're probably best replacing this with Ungoogled Chromium or straight up chromium from xtradeb.)
- Armcord (Discord's desktop client for ARM64 devices.)
- Mesa Zink, with Turnip exposing Vulkan 1.3, and assuming you're on a newer device, OpenGL 4.6.
- KDE Connect, if you wish to use a bluetooth keyboard, and your phone as a trackpad (assuming you are on a tablet like I was)
- Everything already preconfigured and ready to go
- A comfy dark theme for KDE Plasma 6

"You son of a b****, I'm in!" (How do I install?):
- If you haven't installed Termux-x11 (termux-x11-nightly and it's application) and pulseaudio, install them now.
- Download the rootfs and copy it to the home directory of your termux install
- Run:

      proot-distro restore ChironOS2024-RootFSTarball
    
- Wait for the process to finish. This will extract and install the tarball under ubuntu-lts.
- When you log in, run "str" to start up the desktop environment.

For convenience sake, create a startup script.

    termux-x11 :0 &
    pulseaudio --start --load="module-native-protocol-tcp auth-ip-acl=127.0.0.1 auth-anonymous=1" --exit-idle-time=-1
    touch ~/touchgpu
    proot-distro login ubuntu-lts --user main --shared-tmp --bind ~/touchgpu:/proc/bus/pci/devices
    ## At this point, when you exit proot-distro, the below will run, killing Pulseaudio and Termux-X11.
    pkill app_process
    pkill pulseaudio

Congrats, you've installed the RootFS and (assuming you did) created a startup script to easily start it :)

(Note: Pretty much all of the code is NOT mine.)
