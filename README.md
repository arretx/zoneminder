# ZoneMinder Video Copy Script

## Purpose

To consolidate MP4 files created by ZoneMinder in the `events` folder into a single folder for scrubbing in Davinci Resolve.

## Initial Problem

When motion is detected in a camera, ZoneMinder logs an event in a MySQL database.  That event is given an incremental number, regardless of the camera that triggers the event.  The video that's created is saved to a folder that references the event number.

One event may end up in `/events/1/2022-02-19/2345/` where "1" is the camera number and "2345" is the event number, and another event on another camera will become `/events/2/2022-02-19/2346/`, and so on.

So, you end up with hundreds of folders under each day and each camera containing snapshots and MP4 files of the event(s).

The length of the videos recorded are determined by each camera's setting in ZoneMinder.

## 2nd Problem

One cannot simply move files from the ZoneMinder structure without casuing mayhem.  It's best to manage content created by ZoneMinder from ZoneMinder's WebUI.  As I understand, when you delete event folders, the events in the database will also be deleted.  Likewise, if you delete events in the database, ZoneMinder will zorch the folder containing the video.  If that video has value (which is why I'm doing this in the first place) then it might be needed for a video editing project.

Over time, ZoneMinder will chew up massive amounts of disk space, especially if you don't setup a script to delete messages after a certain number of days.  Those "non-incident" security videos are lost forever as they should be.  But every once in a while, there's a gem that catches evidence of something good, or possibly bad.  Either way, we want that footage preserved while also managing storage efficiently.

## Logical Conclusion

Copy all of the videos for a given camera and day to a new location that's conducive to editing video in Davinci Resolve.  From there we can quickly scrub non-events and trim video to contain only the good content, which will take up much less space.

# How it works, currently:

The script, when run in terminal on a Mac (only thing I've tested), will prompt you with an option menu to choose the day for which you want to copy media.  It will then create a folder in the path you've defined in the `config` file for that date, and then prompt you for which camera you want to copy files.  The cameras are also defined in the `config` file.  When you choose a camera, a folder for that camera will be created under the date folder.  After that, it will find all *.MP4 files in the target path and make a text file with a list of its findings.  The final step syncs all of the files in that text file to the target location so they're all in one folder.

