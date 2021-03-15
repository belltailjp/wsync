inotifywait + rsync

### Setup

```bash
$ sudo apt install inotify-tools
$ sudo wget https://raw.githubusercontent.com/belltailjp/wsync/master/wsync -O /usr/local/bin/wsync
$ sudo chmod ugo+x /usr/local/bin/wsync
```

### Usage

```bash
$ wsync --av $PWD xxx@remote:/home/xxx/ --exclude='.git'
```

It watches the target directory (in this case `$PWD`) using inotifywait,
and a modification event is trapped it runs rsync with the arguments specified.


### Troubleshooting

#### inotify watch limitation

In case watching a large directory (many files inside), it may raise the following error.

```bash
Failed to watch /some/directory; upper limit on inotify watches reached!
Please increase the amount of inotify watches allowed per user via `/proc/sys/fs/inotify/max_user_watches'.
```

In such a case, you need to increase the limitation.

```bash
# Check the current limitation (the value depends on the system)
$ cat /proc/sys/fs/inotify/max_user_watches
8192

# Increase the limitation
$ echo fs.inotify.max_user_watches=65536 | sudo tee -a /etc/sysctl.conf
$ sudo sysctl -p

# Check
$ cat /proc/sys/fs/inotify/max_user_watches
65536
```

c.f., https://askubuntu.com/questions/770374

