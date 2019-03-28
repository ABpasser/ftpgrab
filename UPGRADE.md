## 5.1.x > 5.2.0

This release changes the configuration layout as it introduces Webhook notification support. Please read the [Configuration](/doc/configuration/) page before upgrading.

## 5.0.0 > 5.1.0

This release changes the configuration layout as it introduces SFTP support. Please read the [Configuration](/doc/configuration/) page before upgrading.

## > 5.0.0

This release breaks the mechanism of previous releases and is not intended to be upgraded to this one. But you can easily translate your old configuration file with the new format. Also, Docker image has been moved to a dedicated organization on [Docker Hub](https://hub.docker.com/u/ftpgrab) and [Quay](https://quay.io/organization/ftpgrab).

## 4.1 > 4.2.0

```console
$ mv /etc/init.d/ftpgrab /usr/bin/
$ update-rc.d ftpgrab remove
```

If you have a cron, do not forget to change it :

```
0 4 * * * ftpgrab seedbox.conf >/dev/null 2>&1
```

## 4.0 > 4.1

Since the project has been renamed `ftpgrab`, some changes has to be made.

```console
// Rename files and folders
$ mv /opt/ftp-sync/ /opt/ftpgrab
$ mv /var/log/ftp-sync/ /var/log/ftpgrab
$ mv /var/run/ftp-sync/ /var/run/ftpgrab
$ mv /opt/ftpgrab/ftp-sync.conf /opt/ftpgrab/ftpgrab.conf
$ mv /etc/init.d/ftp-sync /etc/init.d/ftpgrab

// Download the latest script and dist config
$ wget https://raw.github.com/ftpgrab/ftpgrab/master/ftpgrab.sh -O /etc/init.d/ftpgrab --no-check-certificate
$ chmod +x /etc/init.d/ftpgrab
$ wget https://raw.github.com/ftpgrab/ftpgrab/master/ftpgrab.conf -O /opt/ftpgrab/ftpgrab.conf --no-check-certificate
```

If you have a cron, do not forget to change the script's name :

```
0 4 * * * cd /etc/init.d/ && ./ftpgrab seedbox.conf >/dev/null 2>&1
```

## 3.x > 4.0

To upgrade from 3.x to 4.x you have to move some files and rename the config and hash file to a custom name like `seedbox.conf` in the below example.

```console
// Move to /opt
$ mv /etc/ftp-sync /opt
$ cd /opt/ftp-sync/

// Create required folders
$ mkdir conf
$ mkdir hash

// Move files
$ mv ftp-sync.conf conf/seedbox.conf
$ mv ftp-sync.txt conf/seedbox.txt

// Download the latest script and dist config
$ wget https://raw.github.com/ftp-sync/ftp-sync/master/ftp-sync.sh -O /etc/init.d/ftp-sync --no-check-certificate
$ chmod +x /etc/init.d/ftp-sync
$ wget https://raw.github.com/ftp-sync/ftp-sync/master/ftp-sync.conf -O /opt/ftp-sync/ftp-sync.conf --no-check-certificate

// Rename log files
$ cd /var/log/ftp-sync/
$ for FILENAME in *; do mv $FILENAME seedbox-$FILENAME; done
```

Next you will have to edit your config file `/opt/ftp-sync/conf/seedbox.conf` :

* Remove lines starting with `LOGS_DIR=`, `PID_FILE=` and `HASH_DIR=`
* Add a new line before `EMAIL_LOG=` with `DIR_DEST="/tmp/seedbox"` (replace `/tmp/seedbox` to your destination folder)
* Add a new line after `DL_RESUME=` with `DL_SHUFFLE=0`

If you have a cron, do not forget to replace the argument to the config file of your choice :

```
0 4 * * * cd /etc/init.d/ && ./ftp-sync seedbox.conf >/dev/null 2>&1
```
