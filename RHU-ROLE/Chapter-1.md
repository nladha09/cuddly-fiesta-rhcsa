# Chapter 1

## Classroom Environment

---

![classroom-env](../images/classroom-env.JPG)

If there is a security issues / bug... etc. with open source software, the power is with whoever is technically capable.

RHEL 8 looks towards fedora to formulate RHEL - supported by Red Hat and throughly tested.

- `ls -l /boot` (long listing against a directory called "boot") vs `ls -all /boot` (as a result of a single dash - interept all letters as a single letter: "a" "l" & a redundant use of "l") vs `ls --all /boot` (with a double dash - we are going to interept the entire word as a single word)

- CTRL + "D" (shortcut to logout)

- fingerprint is located - `vi .ssh/known_hosts` 

- **unconditional** commands `;` (execute all commands regardless of successful completion) vs **conditional** commands `&&` (execute _only_ if the previous command executed successfully)
    - also **conditional** `||` the cmd to the left of the double pipe, _only_ then are we going to execute the command to the right.

- `file` command - looks at contents of file and tells us what type of file it is.
    - `file /etc/issue` - useful in determining type of file we are dealing with

- displaying contents of a file - `cat /etc/fstab` - `tac` is the opposite and shows the bottom of a file. `head` - will show first 10 lines, similiarly `tail -40 /etc/services` will show you the last 40 lines

- `wc -l /etc/services` (counts the lines in a file)

-

