# Chapter 3

## Accessing Network-Attached Storage | Mounting Network-Attached Storage with NFS

---

Mount persistently: To ensure that the NFS share is mounted at boot time, eidt the `/etc/fstab` file to add the mount entry.

```bash
serverb:/share /mountpoint nfs   rw,soft   0  0
```

- `vi /etc/nfs.conf`

---
- `yum -y install autofs`

- `vi /etc/auto.master` - looks for addt'l configuration files

- `/etc/auto.master.d` - This file identifies the base directory used for mount points & identifies the mapping file used for creating the automounts. (ex: /etc/auto.master.d/demo.autofs` (`.autofs`)
     - add the master map entry, in this case, for indirectly mapped mounts:
     `/shares  /etc/auto.demo`
     ^ this entry uses the `/shares` directory as the base for indirect automounts. The `/etc/auto.demo` file contains the aount details. Use an absolute file name. The `auto.demo` file needs to be create before starting the `autofs` service.

- create the mapping files. each mapping file identifies the mount point, mount options, and source location to mount for a set of automounts.
     - `vi /etc/auto.demo`
- the mapping file-naming convention is `/etc/auto.name` where _name_ reflects the content of the map.

#### indirect maps
- the format of an entry is _mount point_. _mount options_, & _source location_
     - basic indirect mapping entry: `work   -rw,sync   serverb:/shares/work`
     - direct maps & indirect maps using wildcards are covered later in this section.

#### direct maps
- direct maps are used to map an NFS share to an existing absolute path mount point.
- to use directly mapped mount points, the master map file might appear as follows:
     - `/- /etc/auto.direct`
- all direct map entries use `/-` as the base directory. in this case, the mapping file that contains the mount details is `/etc/auto.direct`
- the content for the `/etc/auto.direct` file might appear as follows:
     - `/mnt/docs   -rw,sync   serverb:/shares/docs`
- the mount point (or key) is always an absolute path. the rest of the mapping file uses the same structure.

In this example the `/mnt` directory exists, and it is not managed by `autofs`. the full directory `/mnt/docs` will be created & removed by the `autofs` service.

#### indirect wildcard maps
- when an NFS server exports multiple subdirectories within a directory, then the automounter can be configured to access any one of those subdirectories using a single mapping entry.
- continugin the previous example, if `serverb:/shares` exports two or more subdirectories & they are accessible using the same mount options, then the content for the `/etc/auto.demo` file might appear as follows: `-rw,sync   serverb:/shares/&`

- the mount point point (or key) is an asterisk character (*). and the subfirectory on the source location is an (&). Everything else in the entry is the same.

- when a user attempts to access `/shares/work` the key * (which is **work** in this example) replaces the (&) in the source location & `serverb:/shares/work` is mounted. As with the indirect example, the **work** directory is created & removed automatically by `autofs`.