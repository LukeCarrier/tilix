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

## Building

```
$ dub build
```

## Running

Run Tilix directly from the working directory in which it was built:

```
$ ./tilix
```
