FROM pytorch/pytorch:2.1.0-cuda11.8-cudnn8-runtime

WORKDIR /app
COPY ResNet18.py /app/ResNet18.py

RUN pip install torchvision \
    && pip install matplotlib

ENTRYPOINT [ "python", "-u","/app/ResNet18.py" ]