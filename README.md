# libntfs for the Wii U

## Usage
Link the application with:
```
-lntfs -liosuhax
```

## Dependencies
To be able to use libntfs for the Wii u, you need to install the following dependencies:

- [libiosuhax](https://github.com/wiiu-env/libiosuhax)

## Building
```
make wiiu-release
```

## Use the prebuilt files from a Docker image.
The image `wiiuenv/libntfs` on [Docker Hub](https://hub.docker.com/r/wiiuenv/libntfs/) provides a prebuilt library in the `/artifacts` directory. Copy it into your DevkitPPC portlibs folder.  

Example:  
```
COPY --from=wiiuenv/libntfs:20201210 /artifacts $DEVKITPRO
```
