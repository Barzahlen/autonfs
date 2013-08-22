# AutoNFS

AutoNFS is a client-side autofs-free NFS Share Automount-Script, initially designed for Debian Squeeze or derivates.

The initial Github version of AutoNFS v1.2.1 - as is - running quite well and stable in our production environment containing of about 20 NFS Shares and one NFS 3 Server - all with latest Debian Squeeze

## Installation and Usage

The locations of the files are given through the folders in this Repository, so for example the autonfs.sh should reside in /usr/local/bin folder in your NFS-Client

1. Place a copy of the autonfs.sh (actual script) in the desired folder
  - Modify the file by setting the correct `NFSSERVER` and `MOUNTS` vars and optionally `NFSVERS`, `MOUNTOPT` and `INTERVAL`
2. Place a copy of the autonfs (LSB init script) file in the desired folder
  - Make both executable with `chmod +x /<path to the files>`
3. Install the init script for bootup with `update-rc.d autonfs defaults`
4. Start AutoNFS with `service autonfs start` ... the mounted folders will magically appear

AutoNFS will now completly take care of those mounts by checking the NFS Server regularily (`INTERVAL`) if its up or down. If its down, it will unmount the shares after 2 check-failures and will auto-remount them as soon as the server is back up. In that case it will also write it to the syslog.

You can also uncomment all lines starting with `#logger` to get a more verbose syslog output (not needed in production)

## Variables explanation

- `NFSSERVER` - The NFS Server you want to mount something from can be either a single IP or a DNS-Name
- `NFSVERS` - Defines the NFS Protocol (2,3 or 4) to be used for checking the Server and is needed to prevent writing "unknown version" warnings in the syslog
- `MOUNTOPT` - Here you can define the classic NFS Mount options like you would in the fstab (uses good defaults for most scenarios)
- `INTERVAL` - How often should AutoNFS check the Server? (60seconds is a good default but we use 15seconds for our HA Environment)
- `MOUNTS` - This is a space-separated list of shares to mount for ex. `MOUNTS=( "/share1" "/share2" )`, whereas the share name and mountpath must be the same in this version. In a later version i will also support different shares and mount-paths

## Roadmap

- [ ] Support for different share-names and mount-paths
- [ ] A plain Bourne-shell compatible version of autonfs.sh (to be used with different shells than bash)
- [ ] Make the init-script more flexible (different autonfs.sh locations for ex. by using a /etc/default/autonfs file)
- [ ] ...(Ideas?)

## Copyright and License

This little tool was made by Martin Seener (c) 2012, 2013 with the initial idea from JeroenHoek at http://ubuntuforums.org/showthread.php?t=1389291 or https://github.com/jdhoek

Released under the GNU GPLv2
