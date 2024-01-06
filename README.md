# FFmpeg Docker image for Rockchip SoC

This work is based on [Linux Server ffmpeg image](https://docs.linuxserver.io/images/docker-ffmpeg/) and [FFmpeg rkmpp repo from hbiyik](https://github.com/hbiyik/FFmpeg)
It builds FFmpeg with Rockchip MPP library support for hardware accelerated video encoding and decoding on Rockchip SoCs.

Built Docker image can be found here: https://hub.docker.com/r/dlag/ffmpeg-rkmpp

## Usage examples
### Hardware accelerated H.264 encoding
```
docker run --rm -it \
    --device=/dev/rga \
    --device=/dev/dri \
    --device=/dev/dma_heap \
    --device=/dev/mpp_service \
    --device=/dev/video0 \
    --group-add 106 \
    --group-add 44 \
    --group-add 46 \
    --privileged=true \
    -v /mnt/data:/mnt/data \
    ffmpeg-rkmpp \
        -probesize 512M \
        -analyzeduration 512M \
        -g:v 75 \
        -c:v h264_rkmpp \
        -rc_mode VBR -b:v 4M -maxrate 4M -bufsize 8M \
        -crf 25 \
        -c:a copy \
        -c:s copy \
        output.mkv
```
### Hardware accelerated H.265/HEVC encoding
```
docker run --rm -it \
    --device=/dev/rga \
    --device=/dev/dri \
    --device=/dev/dma_heap \
    --device=/dev/mpp_service \
    --device=/dev/video0 \
    --privileged=true \
    -v /mnt/data:/mnt/data \
    ffmpeg-rkmpp \
        -i input.mkv \
        -probesize 512M \
        -analyzeduration 512M \
        -g:v 75 \
        -c:v hevc_rkmpp \
        -crf 25 \
        -c:a copy \
        -c:s copy \
        output.mkv
```
## Build Docker image
```
docker build --platform linux/aarch64 -f Dockerfile.aarch64 -t dlag/ffmpeg-rkmpp:latest .
```