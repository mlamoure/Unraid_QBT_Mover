# Unraid_QBT_Mover
Script to pause torrents based on age and presence on cache prior to invoking the Unraid mover

This script is a modified version of the script written by [bobokun](https://github.com/bobokun).  It's intended for those that want to keep torrent data on the cache drive up until the Mover Tuning plugin threshhold, and when running, pause as few torrents as possible.

Caveats:
* I have not tested with many configurations of the Mover Tuning.  On my system, I use a simple "age" configuration with threshhold amount.
* It's intended for those that also run [QBT_Manage](https://github.com/StuffAnThings/qbit_manage) via docker on Unraid.  Though, you could simply comment this part out or ignore the errors if this does not fit your configuration.
* The script does not guarentee any behavior by the Unraid mover.  It simply encourages the mover to move the torrents that you are pausing.  I've added some informative logging to inform if the torrents that were paused were actually moved or not.

What it does:
* Rather than pausing torrents based on age, the script identifies torrents in QBT that are on the cache, and pauses only those.
* It reads the Mover Tuning configuration file (it will not work if you do not have the Mover Tuning plugin), and calculates the threshhold amount that is configured.  This value is used to pause torrents up to that size, therefore encouraging the mover to move those items off the cache and onto the array.
* It checks to see if mover is running prior to running the pre-mover or post-mover activities.  Sometimes Mover can take a long time and you'll have multiple runs of this script collide.
* It pauses QBT_Manage docker container, if you use it.  This is because QBT_Manage goes a bit crazy while the mover is running, tagging and then untagging torrents with the no_HL tag.  This is because the Unraid mover waits until the end of the run to move hardlinks.
* It tags the torrents in QBT that are on the cache.  I just found it useful to see.

Install: Same as the [Trash Guides](https://trash-guides.info/Downloaders/qBittorrent/Tips/How-to-run-the-unRaid-mover-for-qBittorrent/) except:
* Replace the move.py that you set up from the Trash Guides with the version in this repo.
* Update the configuration section
* Add pip dependencies to your : docker psutil.  Making it: pip3 install qbittorrent-api docker psutil
