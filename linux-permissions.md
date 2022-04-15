# Linux Permissions

## Table of contents

- [BusyBox / Alpine](#busybox--alpine)
- [Most Linux distros](#most-linux-distros)

## BusyBox / Alpine

```sh
addgroup -g 1000 groupname
# -g GID Group id
# -S     Create a system group

adduser -u 1000 -s /bin/sh -G groupname -D username
# -h DIR	Home directory
# -g GECOS	GECOS field
# -s SHELL	Login shell
# -G GRP	Group
# -S		Create a system user
# -D		Don't assign a password
# -H		Don't create home directory
# -u UID	User id
# -k SKEL	Skeleton directory (/etc/skel)
```

## Most Linux distros

```sh
# -g gid | groupname
groupadd -g 1000 humans

# Options:
#   -f, --force                   exit successfully if the group already exists,
#                                 and cancel -g if the GID is already used
#   -g, --gid GID                 use GID for the new group
#   -h, --help                    display this help message and exit
#   -K, --key KEY=VALUE           override /etc/login.defs defaults
#   -o, --non-unique              allow to create groups with duplicate
#                                 (non-unique) GID
#   -p, --password PASSWORD       use this encrypted password for the new group
#   -r, --system                  create a system account
#   -R, --root CHROOT_DIR         directory to chroot into
#   -P, --prefix PREFIX_DIR       directory prefix
#       --extrausers              Use the extra users database
```

```sh
# -u uid | -s shell | -G groups | username
useradd -u 1000 -s /bin/bash -G humans james

# Options:
#       --badnames                do not check for bad names
#   -b, --base-dir BASE_DIR       base directory for the home directory of the
#                                 new account
#       --btrfs-subvolume-home    use BTRFS subvolume for home directory
#   -c, --comment COMMENT         GECOS field of the new account
#   -d, --home-dir HOME_DIR       home directory of the new account
#   -D, --defaults                print or change default useradd configuration
#   -e, --expiredate EXPIRE_DATE  expiration date of the new account
#   -f, --inactive INACTIVE       password inactivity period of the new account
#   -g, --gid GROUP               name or ID of the primary group of the new
#                                 account
#   -G, --groups GROUPS           list of supplementary groups of the new
#                                 account
#   -h, --help                    display this help message and exit
#   -k, --skel SKEL_DIR           use this alternative skeleton directory
#   -K, --key KEY=VALUE           override /etc/login.defs defaults
#   -l, --no-log-init             do not add the user to the lastlog and
#                                 faillog databases
#   -m, --create-home             create the user's home directory
#   -M, --no-create-home          do not create the user's home directory
#   -N, --no-user-group           do not create a group with the same name as
#                                 the user
#   -o, --non-unique              allow to create users with duplicate
#                                 (non-unique) UID
#   -p, --password PASSWORD       encrypted password of the new account
#   -r, --system                  create a system account
#   -R, --root CHROOT_DIR         directory to chroot into
#   -P, --prefix PREFIX_DIR       prefix directory where are located the /etc/* files
#   -s, --shell SHELL             login shell of the new account
#   -u, --uid UID                 user ID of the new account
#   -U, --user-group              create a group with the same name as the user
#   -Z, --selinux-user SEUSER     use a specific SEUSER for the SELinux user mapping
#       --extrausers              Use the extra users database
```
