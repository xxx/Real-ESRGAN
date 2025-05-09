FROM busybox AS builder

WORKDIR /models
RUN wget "https://github.com/xinntao/Real-ESRGAN/releases/download/v0.1.0/RealESRGAN_x4plus.pth"
RUN wget "https://github.com/xinntao/Real-ESRGAN/releases/download/v0.2.1/RealESRGAN_x2plus.pth"
RUN wget "https://github.com/xinntao/Real-ESRGAN/releases/download/v0.1.1/RealESRNet_x4plus.pth"
RUN wget "https://github.com/xinntao/Real-ESRGAN/releases/download/v0.2.2.4/RealESRGAN_x4plus_anime_6B.pth"
RUN wget "https://github.com/xinntao/Real-ESRGAN/releases/download/v0.2.5.0/realesr-general-x4v3.pth"
RUN wget "https://github.com/xinntao/Real-ESRGAN/releases/download/v0.2.5.0/realesr-animevideov3.pth"
RUN wget "https://github.com/xinntao/facexlib/releases/download/v0.1.0/detection_Resnet50_Final.pth"
RUN wget "https://github.com/xinntao/facexlib/releases/download/v0.2.2/parsing_parsenet.pth"

WORKDIR /gfpgan
RUN wget "https://github.com/TencentARC/GFPGAN/releases/download/v1.3.0/GFPGANv1.3.pth"

FROM python:3.11-slim

RUN apt-get update && apt-get -y install \
    bash ffmpeg curl git

WORKDIR /real-esrgan
COPY requirements.txt .
RUN pip3 install --upgrade pip
RUN pip3 install -r requirements.txt

COPY . .
COPY --from=builder /models/* weights
RUN mkdir weights/gfpgan
COPY --from=builder /gfpgan/* weights/gfpgan
RUN python3 setup.py develop
COPY --chmod=755 docker/inference_img docker/inference_video /usr/local/bin

WORKDIR /host
ENTRYPOINT ["/bin/bash"]

ENV NVIDIA_DRIVER_CAPABILITIES=all
