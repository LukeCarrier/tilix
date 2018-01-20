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
        gtk+3 \
        vte3
```

## Building

```
$ dub build
```

## Running

Run Tilix directly from the working directory in which it was built:

```
$ ./tilix
```
