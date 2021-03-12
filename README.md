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

It watches the target directory (in this case `$PWD`) until inotifywait traps a modification event,
then runs rsync with the arguments specified.
Afterwards, it waits the modification again.

rsync only runs when a modification is made in the sync source directory,
which significantly reduces remote server stress.

