# Building Tilix on macOS

Note: this build is a work in progress, doesn't work right now and certainly
isn't pretty. It will need a lot of work before being considered for inclusion
upstream.

## Dependencies

Install the build tools ([`dmd` compiler](https://dlang.org/dmd-osx.html),
[`dub` build tool](https://code.dlang.org/download)):

```
$ brew install dmd dub
```

Install necessary libraries:

* [XQuartz](https://www.xquartz.org/), since X11 is not included in macOS any
  more
* [GTK+](https://www.gtk.org/), for the UI
* [VTE](https://github.com/GNOME/vte), for the terminal widget

```
$ brew install \
        caskroom/cask/xquartz \
        desktop-file-utils \
        gettext \
        gtk+3 \
        vte3
```

And finally some tools to make our lives easier:

```
$ brew install xmlstarlet
```

## Building

```
$ dub build
```

## Installation

Ensure required directories are on `$PATH`:

```
$ export PATH="$PATH:$(brew --prefix gettext)/bin"
```

Help the install script find `gtk-update-icon-cache`:

```
$ sudo ln -s /usr/local/bin/gtk{3,}-update-icon-cache
```

If you're having trouble with `install.sh` failing to locate binaries, try
inspecting the list of directories. Some Homebrew packages include version
numbers in filenames:

```
$ echo $PATH | tr : $"\n"
```

Then install:

```
$ ./install.sh prefix
```

## Running

Prepare a local dbus session, so that we're able to run Tilix without globally
installing GSettings schema:

```
$ export DBUS_SESSION_BUS_ADDRESS="/tmp/dbus/dbus.$UID.usock"
$ mkdir -p "$(dirname $DBUS_SESSION_BUS_ADDRESS)"
$ export XDG_DATA_DIRS="export XDG_DATA_DIRS=$(pwd)/prefix/share"
$ cp $(brew --prefix dbus)/share/dbus-1/session.conf prefix/share/dbus-1/session.conf
$ xmlstarlet ed \
        -L -u /busconfig/listen -v "unix:path=${DBUS_SESSION_BUS_ADDRESS}" \
         prefix/share/dbus-1/session.conf
$ exec dbus-launch --config-file prefix/share/dbus-1/session.conf $SHELL
```

Run Tilix directly from the working directory in which it was built:

```
$ ./prefix/bin/tilix
```

## Useful resources

* [dbus on macOS](https://github.com/zbentley/dbus-osx-examples/tree/master/installation)
