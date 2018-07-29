# openssh-initramfs

An initramfs package for Debian allowing us to use OpenSSH instead of the limited Dropbear for remote unlock.

For now, while a [private initiative has been started](https://github.com/pts/pts-dropbear), there is still no official implementation of ed25519 for Dropbear.
The OpenSSH implementation also avoids to have to convert OpenSSH keys to dropbear and allows to have [stronger algorithms](https://stribika.github.io/2015/01/04/secure-secure-shell.html).

## To build this package

Install dependencies:
```
apt install devscripts dh-exec
```

Clone, prepare and build sources:
```
git clone https://github.com/wget/openssh-initramfs.git
cd openssh-initramfs/
tar -cvzf openssh-initramfs_1.1.orig.tar.gz openssh-initramfs-1.1/usr/
mv openssh-initramfs-1.1/usr/ openssh-initramfs-1.1/debian/
cd openssh-initramfs-1.1/
debuild -us -uc
```

Note: There is no need to regenerate an archive each time you make changes to this repo. When you will rebuild the package to do some tests for example, the files from `debian/usr` are the ones that will be taken into account. The archive is only needed because the Debian helper tools assume the source code has been downloaded from somewhere. If there is no archive, the build process will fail even if we override in order to continue:
```
$ debuild -us -uc
This package has a Debian revision number but there does not seem to be
an appropriate original tar file or .orig directory in the parent directory;
(expected one of openssh-initramfs_1.1.orig.tar.gz, openssh-initramfs_1.1.orig.tar.bz2,
openssh-initramfs_1.1.orig.tar.lzma,  openssh-initramfs_1.1.orig.tar.xz or openssh-initramfs-1.1.orig)
continue anyway? (y/n) y
[...]
dpkg-source: error: can't build with source format '3.0 (quilt)': no upstream tarball found at ../openssh-initramfs_1.1.orig.tar.{bz2,gz,lzma,xz}
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
