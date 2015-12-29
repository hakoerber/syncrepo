# Syncrepo

This utility can be used to create a local yum package repository and keep it up to date, for example by using `cron(8)`

## Configuration file

Syncrepo reads a file specifiying some parameters in JSON syntax (see `repos.example`). This file
must contain the following keys:

`base` points to the base path on the local machine that all data is written to. This is the
same path that would be exported over HTTP or FTP.

`repos` contains mappings, specifiying which repository to get from which mirror. The keys are
paths relative to the "base" path, and the values are URLs to `rsync(1)` capable mirrors.

For example, when using `repos.example`, `mirror.example.com/centos/7/os` would be syncted to `/srv/www/packages/centos/7/os`.

## Scheduling syncs with cron

Adding the following line to `/etc/crontab` would sync the mirror at 23:00 every day:

    0 23 * * * root /usr/local/bin/syncrepo --config /etc/syncrepo.conf >>/var/log/syncrepo.log 2>&1

The time was chosen because `yum-cron(8)` runs some time
after midnight by default, and gives rsync enough time to update the mirror.

## Requirements

* Python 3 <= 3.4 (3.5 changed the `subprocess` module)
* Non-ancient Rsync

## License

Apache License 2.0
