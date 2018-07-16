# openssh-initramfs

An initramfs package for Debian allowing us to use OpenSSH instead of the limited Dropbear for remote unlock.

For now, while a [private initiative has been started](https://github.com/pts/pts-dropbear), there is still no official implementation of ed25519 for Dropbear.
The OpenSSH implementation also avoids to have to convert OpenSSH keys to dropbear and allows to have [stronger algorithms](https://stribika.github.io/2015/01/04/secure-secure-shell.html).

## To build this package

```
git clone https://github.com/wget/openssh-initramfs.git
cd openssh-initramfs/
tar -cvzf openssh-initramfs_1.0.orig.tar.gz openssh-initramfs-1.0/usr/
mv openssh-initramfs-1.0/usr/ openssh-initramfs-1.0/debian/
cd openssh-initramfs-1.0/
debuild -us -uc
```

## Test from dev environment

In order to don't be locked outside, or cut one's branch tree, clone this repo on a development machine outside of your test machine, mount the folder using sshfs and copy important files using the following commands (assuming the repo is sshfs mounted in /mnt):

```
cp /mnt/usr/share/initramfs-tools/scripts/init-premount/openssh /usr/share/initramfs-tools/scripts/init-premount/openssh &&\
cp /mnt/usr/share/initramfs-tools/scripts/init-bottom/openssh /usr/share/initramfs-tools/scripts/init-bottom/openssh &&\
cp /mnt/usr/share/initramfs-tools/hooks/openssh /usr/share/initramfs-tools/hooks/openssh &&\
cp /mnt/usr/share/initramfs-tools/conf-hooks.d/openssh /usr/share/initramfs-tools/conf-hooks.d/openssh
```

## Future improvement

Find a way to make that damn PAM modules work.

We tried to copy the files from the packages `libpam-runtime`, `libpam-modules`, `libpam-modules-bin`, files contained in `/etc/pam.d/*` and thos ein `/lib/XXX-linux-gnu/pam*` without much success, the system was still complaining with errors like `"Failed none for invalid user root"`.

## License

GPL v2 in order to facilitate potential inclusion inside Debian as official package.
