progress-md5-copy
=================

Usage: `progress-md5-copy SOURCE [DEST]`

Copies `SOURCE` to `DEST`, while displaying progress and writing an MD5sum to `SOURCE.md5`.

`SOURCE` must be a regular file.

`DEST` can be a directory or a filename. (Defaults to current directory.)

If `SOURCE.md5` exists then it is checked against `SOURCE` instead of creating a new checksum.

The script uses the `tee` command to compute the MD5sum while performing the copy at the same time. `SOURCE` is only read once. (Good for giant files.)

**Note: When an MD5sum is found and checked during a copy, it is only checked against the content of SOURCE. DEST is not read back to make sure it was written correctly.**

Requirements
------------

Should work on any Unix system. Requires the commands `pv` and `md5sum`.

Install
-------

```bash
$ mkdir -p ~/bin
$ cd ~/bin
$ wget https://raw.github.com/bkidwell/progress-md5-copy/master/progress-md5-copy
$ chmod +x progress-md5-copy
```

Example Usage
-------------

Suppose you want to copy a virtual machine from one host to another via a thumbdrive.

On the source machine, you would mount the thumbdrive and then do:

```bash
$ ~/bin/progress-md5-copy ~/Transfer/important-vm.tar.gz /media/thumbdrive
Copying
   "~/Transfer/important-vm.tar.gz"
to
   "/media/thumbdrive/important-vm.tar.gz"
and creating MD5sum in "/media/thumbdrive/important-vm.tar.gz.md5"

  3GB 0:00:30 [ 100MB/s] [===================================>] 100%
```

Unmount the thumbdrive, move to the destination machine and mount it there. Then:

```bash
$ ~/bin/progress-md5-copy /media/thumbdrive/important-vm.tar.gz ~/Transfer
Copying
   "/media/thumbdrive/important-vm.tar.gz"
to
   "~/Transfer/important-vm.tar.gz"
and checking existing MD5sum "/media/thumbdrive/important-vm.tar.gz.md5"

  3GB 0:00:30 [ 100MB/s] [===================================>] 100%
-: OK
```

If the checksum didn't match, the last line would say something other than `OK`.

