# Network Attached Storage

Recommended Setup as per [Perfect Media Server](https://perfectmediaserver.com).
Highly recommend watching [PMS Part 1](https://www.youtube.com/watch?v=Yt67zz9p0FU).

- MergerFS
- SnapRAID

## MergerFS

MergerFS allows mounting any size disk. It works at the file layer and does not care what filesystem (btrfs, xfs, ext4, zfs, etc) is used under the hood.
This means easy plug-and-play harddrive re-configuring and moving.

## SnapRAID

Data Protection / Fault Tolerance
It uses one drive as the "parity" drive.
It shuvs parity information to an entire dedicated parity disk.
Its done on a snapshot basis, not realtime.

NOT good for high count smol files.
(i guess maybe not necessary)
