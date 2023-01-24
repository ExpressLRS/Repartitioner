# DO NOT FLASH THIS TO A RECEIVER!
If you do this and the receiver is an ESP8285 based receiver, which most are, you will soft-brick your receiver and will have to remove it from you build and re-flash it with an FTDI adapter. YOU HAVE BEEN WARNED!

# What is this?

This is a small firmware that resolves the dreaded "Bad Size Given" problem in some ExpressLRS TX modules.
There are reports that this can also solve problems with the "Not Enough Space" error. 

# I've got the "Bad Size Given" problem, how do I use this to fix it?

Start the WiFi updater on your TX module. Instead of uploading the 3.x.x firmware, where you get the "Bad Size Given" error, upload this small firmware instead. It will say that the target does not match, just select "Flash Anyway". The module will reboot a couple of times and then you should be able to go back to the WiFi updater page and upload the 3.x.x firmware without an issue.

# What does it do?

To explain what this does, we need to understand the problem.
Some manufacturers have flashed their modules using a different method to what we, the ExpressLRS devs, use in PlatformIO and EspressLRS-Configurator. They have used something like NodeMCU Flasher and left it at the default partition layout. This causes the problem because the default layout has much smaller OTA partitions than what ExpressLRS needs and is using by default.

Version 3 of ExpressLRS has a much larger binary than previous versions, because we have moved to a unified binary for Espressif devices and this does not fit in the default OTA partitions, hence the "Bad Size Given" error, meaning the binary is too big for the partition.

Now that you know why the problem exists. So, what does this do to fix it?

This small firmware flashes the partition table with the correct sizes for ExpressLRS. It then relocates the old ExpressLRS firmware if it was in OTA partition 1 to the correct place. If it was in OTA partition 0, then it doesn't need to move it. It then resets the boot partition to the existing ExpressLRS partition and reboots.

Now the partition sizes are correct, and you can flash newer versions of ExpressLRS without the error.
