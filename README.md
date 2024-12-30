
# Replacing my Drobo 5N with a Raspberry Pi solution

  
  

## Old Setup and Problem

  

I had a Drobo 5N with 5 drives. 4x6TB drives and 1x4TB drive

  

This had been functioning really well since I bought it in August 2016. The odd drive had failed, which I expected, but the Drobo had been rock solid at recovering when a new drive was inserted and never lost any data.

  

Cut to now, it appears the Drobo chassis is dead. At first I had an instance of all the lights being RED. Reboot and all was fine. Then all the lights would go solid yellow. A reboot would get it back working for about an hour until the yellow lights all came back.

Initially I though the SSD I was using as a cache drive might be broken but removing this made no difference. In the end all that happens on the Drobo is I get a single red light on Bay 0 and the dashboard asks for drives to be inserted.

  

Some googling did suggest that it could be a PSU issue. A new PSU showed no change.

  

Very annoying and this isn't really a failure scenario I had planned for. This whole thing is made double painful due to Drobo going out of business. So there are no new units to buy as a replacement. Where this really becomes a problem is that the Drobo file storage system "Beyond RAID" is proprietary. So data recovery isn't simple but it does look like this is a solvable problem. Just an added expense and no doubt it will be time consuming.

## The Plan

Obviously a replacement is needed and probably the quickest and less faff option would be to just buy a Synology NAS with new drives and be done with it. This is just the most expensive option available.

There is NO option that allows me to plug in the current drive array to recover data and just move on.

As there is going to be faff lets try and add some objectives to this new setup. I am a big fan of Raspberry Pi's. I just think they are cool, they are extremely well supported and have a huge amount of community support. I already have some PI's running various web applications like PiHole and Plex. So there could be an option to consolidate.

- Use a Raspberry Pi
- Learn new stuff
- Future proof the system
	- Not use anything proprietary
	- Ensure that any data can be plugged into a different system for immediate recovery, assuming drive is not broken
	- Not have any single dependency on specific hardware
- Keep with the "Use any Drive" approach from the Drobo. Although this will require a little more admin

A lot of the inspiration for my plan comes from Jeff Geerling and his Pi Nas project 

[Arm NAS](https://github.com/geerlingguy/arm-nas) 
### Hardware


  - (SBC) [Raspberry Pi 5](https://www.raspberrypi.com/products/raspberry-pi-5/)
  - (HAT) [Radxa Penta SATA HAT for Pi 5](https://arace.tech/products/radxa-penta-sata-hat-up-to-5x-sata-disks-hat-for-raspberry-pi-5)
  - (microSD) [Raspberry Pi A2 128GB](https://thepihut.com/products/noobs-preinstalled-sd-card)
  - (Power) [12V 10A AC adapter](https://amzn.eu/d/dBCyL4a)
  - (HDD) [2x Seagate IronWolf Pro 12 TB](https://amzn.eu/d/aOSbels)
  - Some Sort of SATA extension cables

### Case

Yet to be found but I think it will end up being 3D printed


### Software

[Open Media Vault ](https://www.openmediavault.org/) 

After going round and round this seems the best choice for what I want to be able to achieve with the disks. This also has a bonus of being able to run docker containers. So this should help consolidate the other apps I have running on the network and create a cleaner recovery scenario as I believe the containers are easier to backup. Another bonus is that USB connected drives should also work, which is a limitation on some other RAID operating system like UNRAID

In the research I have done. In order to get the most Drobo like hard drive setup I can use both MergerFS and SnapRAID. SnapRAID seems designed to work with MergerFS, which is great.

MergerFS essentially joins a bunch of disks together into a single storage volume. Much like an old school JBOD but a bit more sophisticated. It appears you can configure how data is written to the disks, so you can spread the data around to maintains even free space across the drives.

Then SnapRAID is used to create a parity drive for all of the data in the MergerFS volume. 
This is very similar to how the Drobo would work. The main difference I can see is that SnapRAID is not instant and runs on a timed job. Given that my data changes very slowly I don't see this as an issue and I can force an update if and when it is needed.

The drives will use the EXT4 file system. This ensures that any drive (assuming it works) can be plugged into other system and the data read.

# Problems to solve

My Drobo array was about 20TB and I would like to try and recover most of this but i dont want to buy loads of new drives. 
My hope is that the 2 12tb drives I am buying can be setup without parity protection while data is recovered. I then want to add 3 of the 6TB drives I have into the system and then switch on SnapRAID to add the parity protection back.

I also need to solve the problem of how to securely house the Raspberry Pi and the hard drives.


# Drobo data recovery

Reading lots of Drobo posts on Reddit it seems that the best approach is to use [UFS Explorer](https://www.ufsexplorer.com/ufs-explorer-raid-recovery/) to recover the data. A few people have had success with this and the software isn't massively expensive. Just a pain.
What is an extra pain is that for any hope of the recovery working ALL of the Drobo drives have to be connected to a single computer that then runs the UFS software. Like most people. I dont easily have the capability to connect up 5 HDD's to my system. I am going to try out some 3.5" HDD docking USB hubs off of amazon and see if they work. 


# Installation

Will update these as the build happens


