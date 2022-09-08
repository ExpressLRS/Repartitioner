# What is this?

This is a small firmware that resolves the dreaded "Bad Size Given" problem in some ExpressLRS TX modules.
There are reports that this can also solve problems with the "Not Enough Space" error. 

# I've got the "Bad Size Given" problem, how do I use this to fix it?

Start the wifi updater on your TX module, instead of uploading the 3.x firmware, where you get the "Bad Size Given" error, upload this small firmware instead. It will say that the target does not match, just select "Flash Anyway". The module will reboot a couple of times and then you should be able to go back to the wifi updater page and upload the 3.x.x firmware without issue.

# What does it do?

To explain what this does, we need to understand the problem.
Some manufacturers have flashed their modules using a different method to what we, the ExpressLRS devs use in PlatformIO and EspressLRS-Configurator. They have used somthing like NodeMCU Flasher and left the default partition layout. This is causes the problem becasue the default layout has much smaller OTA partitions that what ExpressLRS is using.

Version 3 of ExpressLRS has a much larger binary that version 2 because we have moved to a unified binary for Espressif devices and this does not fit in the default OTA partitions, hence the "Bad Size Given" error, meaning the binary is too big for partition.

Now that you know why the problem exists. So what does this do to fix it?

This firmware is a very small firmware that flashes the partition table with the correct ope for ExpressLRS. It then relocates the old ExpressLRS firmware if it was in OTA parition 1 to the correct place. If it was in OTA partition 0, then it doesn't need to move it. It then resets teh boot partition to the existing ExpressLRS partition and reboots.

Now the partition sizes are correct and you can flash newer versions of ExpressLRS without the error.
