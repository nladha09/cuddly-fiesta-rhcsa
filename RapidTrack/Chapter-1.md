# Chapter 1

## Accessing Systems & Obtaining Support

---

- `redhat-support-tool`

## Managing Local Users & Groups | Describing User & Group Concepts

---

- The only difference b/w your Primary Group (can only be one) & Supplementary Groups (can have many)
    - This is the group listed by GID number in the `/etc/passwd` file & by default this is the group that will own new files created by the user.

- using `su - <username>` is the switch user cmd
    - using `su -` will switch to `root` identity & its env.
    - using `su` (w/o dash) will keep your same env. but switch to `root` in that shell / env

- configuring no password for service accounts:
![sudo-nopasswd](../images/sudo-nopasswd.JPG)

