# 安装安装 Pytorch + YOLOv5

## (〇) 版本

### 1. 硬件

* CPU：Inter i7 9750H
* GPU：NVIDIA GeForce GTX1660Ti

### 2. 固件

* 显卡驱动：NVIDIA driver 460

### 3. 软件

* CUDA：10.2
* cuDNN：8.2.2



## (一) 安装 Pytorch

### 1. 下载链接

* 各版本下载链接：https://download.pytorch.org/whl/torch_stable.html

* CUDA 支持向下兼容——torch 没有对应 CUDA 11.2 的版本

### 2. 安装 torch

* 下载链接：[cu102/torch-1.8.1%2Bcu102-cp37-cp37m-linux_x86_64.whl](https://download.pytorch.org/whl/cu102/torch-1.8.1%2Bcu102-cp37-cp37m-linux_x86_64.whl)

* ```
  pip3 install torch-1.8.1+cu102-cp37-cp37m-linux_x86_64.whl 

### 3. 安装 torchvision

* 下载链接：[cu102/torchvision-0.9.1-cp38-cp38-linux_x86_64.whl](https://download.pytorch.org/whl/cu102/torchvision-0.9.1-cp38-cp38-linux_x86_64.whl)

* ```
  pip3 install torchvision-0.9.1-cp37-cp37m-linux_x86_64.whl
  ```

### 4. 验证是否安装成功

* ```
  python
  ```

* ```
  import torch
  ```

* ```
  import torchvision
  ```

* ```
  device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")

* ```
  print(device)
  ```

* 输出 "cuda:0" 则安装成功



## (二) 安装 YOLOv5

### 1.下载链接

* 下载链接：https://github.com/ultralytics/yolov5

### 2. 安装依赖

* ```
  pip3 install -r requirements.txt

* 如果网速不行，则单独下载

* ```
  # YOLOv5 🚀 requirements
  # Usage: pip install -r requirements.txt
  
  # Base ------------------------------------------------------------------------
  gitpython
  ipython  # interactive notebook
  matplotlib>=3.2.2
  numpy>=1.18.5
  # opencv-python>=4.1.1
  Pillow>=7.1.2
  psutil  # system resources
  PyYAML>=5.3.1
  requests>=2.23.0
  scipy>=1.4.1
  torch>=1.7.0
  torchvision>=0.8.1
  thop>=0.1.1  # FLOPs computation
  tqdm>=4.64.0
  # protobuf<=3.20.1  # https://github.com/ultralytics/yolov5/issues/8012
  
  # Logging ---------------------------------------------------------------------
  tensorboard>=2.4.1
  # clearml>=1.2.0
  # comet
  
  # Plotting --------------------------------------------------------------------
  pandas>=1.1.4
  seaborn>=0.11.0
  
  # Export ----------------------------------------------------------------------
  # coremltools>=6.0  # CoreML export
  # onnx>=1.9.0  # ONNX export
  # onnx-simplifier>=0.4.1  # ONNX simplifier
  # nvidia-pyindex  # TensorRT export
  # nvidia-tensorrt  # TensorRT export
  # scikit-learn<=1.1.2  # CoreML quantization
  # tensorflow>=2.4.1  # TF exports (-cpu, -aarch64, -macos)
  # tensorflowjs>=3.9.0  # TF.js export
  # openvino-dev  # OpenVINO export
  
  # Deploy ----------------------------------------------------------------------
  # tritonclient[all]~=2.24.0
  
  # Extras ----------------------------------------------------------------------
  # mss  # screenshots
  # albumentations>=1.0.3
  # pycocotools>=2.0  # COCO mAP
  # roboflow
  # ultralytics  # HUB https://hub.ultralytics.com
  
* 高版本 opencv-python 需要安装 opencv-contrib-python

* pandas 版本不能过高，否则会因为 coremltools 不支持高版本 numpy 而自动卸载高版本 numpy 安装低版本numpy 而导致 pandas 无法使用

* onnx 需要 protobuf

* wandb 需要 pathtools、sentry_sdk

  * sentry_sdk 需要 urllib3——进而显示需要 pathlib、ruamel.yaml、ruamel.yaml.clib

* tensorboard 需要 tensorboard-plugin-wit、absl-py、Markdown、grpcio、google_auth、rsa
* albumentations 需要 opencv_python_headless

### 3. 下载预训练权重文件

* 下载链接：https://github.com/ultralytics/yolov5/releases

### 4 测试安装成功

* 出现下图情况则表示安装成功
* 直接测试
* ![detect](../../Pictures/YOLOv5/detect_pic.png)

* 将测试数据源改为摄像头
* ![detect_0](../../Pictures/YOLOv5/detect_0.png)
