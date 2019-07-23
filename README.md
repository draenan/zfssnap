# zfssnap - Snapshot a ZFS file system with rolling snapshots

`zfssnap` was created to manage the creation and rotation of ZFS snapshots for
backup purposes.  It pre-dated the creation of `sysutils/zfstools` by about
four months.  Written to be compatible with FreeBSD 8+ and Solaris 10,
although it was never used with Solaris.

It was retired in June 2014 once I found out about `sysutils/zfstools`.

## Usage

```
zfssnap -s snapname [-n maxsnaps] filesystem
```

* `-s`: Snapshot name (eg "weekly")
* `-n`: Number of snapshots to keep (optional)
* `filesystem`: Name of ZFS file system to snapshot.

`filesystem` included both the file system itself, and any children.

The `-n` argument could be used to trim the number of saved snapshots.
Generally you would be using "24" for hourly snapshots, "7" for daily, "4" for
weekly, and "12" for monthly.  Assuming the pool you wanted to snapshot was
called "datapool", you would then put the following into `/etc/crontab`:

```
@hourly     root    /usr/local/sbin/zfssnap -s hourly  -n 24 datapool
@daily      root    /usr/local/sbin/zfssnap -s daily   -n  7 datapool
@weekly     root    /usr/local/sbin/zfssnap -s weekly  -n  4 datapool
@monthly    root    /usr/local/sbin/zfssnap -s monthly -n 12 datapool
```

