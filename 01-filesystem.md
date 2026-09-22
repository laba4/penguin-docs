# Filesystem Layout and Navigation

## The core idea

Linux has **one single directory tree** that starts at `/` ("root").
There are no drive letters like `C:`. Extra disks and USB sticks get attached
(mounted) as folders somewhere inside the tree.

## Important directories

| Directory   | What lives there                          | When I look there                 |
|-------------|-------------------------------------------|-----------------------------------|
| `/etc`      | Configuration files                       | To change how something behaves   |
| `/var/log`  | Log files                                 | When something is broken          |
| `/home`     | Personal folders of normal users          | User files                        |
| `/root`     | Home folder of the root (admin) user      | Admin's own files                 |
| `/usr/bin`  | Most programs / commands                  | To find where a command lives     |
| `/tmp`      | Temporary files, cleaned up regularly     | Scratch space                     |
| `/opt`      | Extra software from vendors               | Third-party applications          |
| `/boot`     | Kernel and files needed to start up       | Boot problems                     |
| `/dev`      | Hardware devices, shown as files          | Disks, terminals                  |
| `/proc`     | Live info about the running system        | CPU, memory, processes            |

## Commands

| Command                    | What it does                                   |
|----------------------------|------------------------------------------------|
| `pwd`                      | Show where I am                                |
| `ls`                       | List folder contents                           |
| `ls -l`                    | Long list with details                         |
| `ls -a`                    | Include hidden files (plus `.` and `..`)       |
| `ls -lh`                   | Human-readable sizes (K, M, G)                 |
| `ls -lSh /var/log`         | Sort by size, largest first human-readable     |
| `cd /path`                 | Go to a folder                                 |
| `cd` or `cd ~`             | Go home                                        |
| `cd ..`                    | One level up                                   |
| `cd -`                     | Back to the previous folder                    |
| `cat file`                 | Print a whole file                             |
| `less file`                | Read page by page (`/word` search, `q` quit)   |
| `head -5 file` / `tail -5 file`  | First / last 5 lines                     |
| `wc -l file`               | Count lines                                    |
| `file something`           | What kind of file is this?                     |
| `which command`            | Where is this **command**?                     |
| `find / -name "x"`         | Where is this **file**? (use `sudo` for all)   |
| `hostnamectl`              | Show / set the hostname                        |

## Reading `ls -l`

```
-rw-r--r--. 1 root root 4521 Sep 22 10:14 messages
```

| Part           | Meaning                                         |
|----------------|-------------------------------------------------|
| `-`            | Type: `-` file, `d` directory, `l` link         |
| `rw-r--r--`    | Permissions                                     |
| `1`            | Number of hard links                            |
| `root root`    | Owner and group                                 |
| `4521`         | Size in bytes                                   |
| `Sep 22 10:14` | Last modified                                   |
| `messages`     | Name                                            |

## "Everything is a file"

`/proc` isn't stored on disk. The kernel creates its files live, so even
system info can be read like a normal file:

```bash
grep -c processor /proc/cpuinfo   # number of CPUs
head -1 /proc/meminfo             # total RAM
```

Friendlier tools that read the same data: `nproc`, `lscpu`, `free -h`.

## Lessons learned

- `which` only finds **commands** in my `PATH`. It can't find config files.
  For any file, use `find`:
  `sudo find / -name sshd_config` &rarr; `/etc/ssh/sshd_config`
- Config files almost always live in `/etc`.
- `/etc/passwd` has many more accounts than real users. Most are
  **system accounts** that services run as (e.g. `sshd`, `apache`).