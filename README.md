<div align="center">

Depth Anything TensorRT
===========================

[![python](https://img.shields.io/badge/python-3.10.12-green)](https://www.python.org/downloads/release/python-31012/)
[![cuda](https://img.shields.io/badge/cuda-11.6-green)](https://developer.nvidia.com/cuda-downloads)
[![trt](https://img.shields.io/badge/TRT-8.6-green)](https://developer.nvidia.com/tensorrt)

<div align="left">

Depth estimation is the task of measuring the distance of each pixel relative to the camera. This repo provides a TensorRT implementation of the [Depth-Anything](https://github.com/LiheYoung/Depth-Anything) depth estimation model in both C++ and Python, enabling efficient real-time inference.

<p align="center">
  <img src="assets/davis_dolphins_result.gif" height="225px" width="720px" />
</p>

## ⏱️ Performance

The inference time includes the pre-preprocessing and post-processing stages:
| Device          | Model | Model Input (WxH) |  Image Resolution (WxH)|Inference Time(ms)|
|:---------------:|:------------:|:------------:|:------------:|:------------:|
| RTX4090        | Depth-Anything-S  |518x518  |  1280x720    | 3     |
| RTX4090        | Depth-Anything-B  |518x518  |  1280x720    | 6     |
| RTX4090        | Depth-Anything-L  |518x518  |  1280x720    | 12    |


> [!NOTE]  
> The inference was conducted using `FP16` precision, with a warm-up period of 10 frames, and the reported time corresponds to the last inference.


## 🚀 Quick Start

#### C++

``` shell
Usage: depth-anything-tensorrt.exe <engine or onnx model> <input image or video>

Example:
# infer image
depth-anything-tensorrt.exe depth_anything_vitb14.engine test.jpg
# infer folder(images)
depth-anything-tensorrt.exe depth_anything_vitb14.engine data
# infer video
depth-anything-tensorrt.exe depth_anything_vitb14.engine test.mp4 # the video path
```

#### Python

```
cd depth-anything-tensorrt/python

# infer image
python trt_infer.py --engine <path to trt engine> --img <single-img> --outdir <outdir> [--grayscale]
```

## 🛠️ Build

#### C++

Refer to our [docs/INSTALL.md](https://github.com/spacewalk01/depth-anything-tensorrt/blob/main/docs/INSTALL.md) for C++ environment installation.

#### Python

``` shell
cd <tensorrt installation path>/python
pip install cuda-python
pip install tensorrt-8.6.0-cp310-none-win_amd64.whl
pip install opencv-python
``` 

## 🤖 Model Preparation

1. Download the pretrained [model](https://huggingface.co/spaces/LiheYoung/Depth-Anything/tree/main/checkpoints) and install [Depth-Anything](https://github.com/LiheYoung/Depth-Anything):
   ```bash
   git clone https://github.com/LiheYoung/Depth-Anything
   cd Depth-Anything
   pip install -r requirements.txt
   ```

2. Copy and paste `dpt.py` in this repo to `<depth_anything_installpath>/depth_anything` folder. Note that I've only removed a squeeze operation at the end of model's forward function in `dpt.py` to avoid conflicts with TensorRT.
3. Export the model to onnx format using `export_to_onnx.py`. You will get an onnx file named `depth_anything_vit{}14.onnx`, such as `depth_anything_vitb14.onnx`.
   
## 👏 Acknowledgement

This project is based on the following projects:
- [Depth-Anything](https://github.com/LiheYoung/Depth-Anything) - Unleashing the Power of Large-Scale Unlabeled Data.
- [TensorRT](https://github.com/NVIDIA/TensorRT/tree/release/8.6/samples) - TensorRT samples and api documentation.
