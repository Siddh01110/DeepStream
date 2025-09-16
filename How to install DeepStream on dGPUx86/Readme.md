🚀 DeepStream SDK Installation on Discrete GPU Systems

This guide explains how to install NVIDIA DeepStream SDK on a discrete GPU (dGPU) supported system from scratch. It also covers prerequisite dependencies, compatibility checks, and running DeepStream sample applications.

📌 1. Introduction

NVIDIA DeepStream SDK is a streaming analytics toolkit for AI-based video and image understanding. It enables you to build optimized pipelines for real-time video analytics on NVIDIA GPUs.

This guide focuses on installation for discrete GPU systems (not Jetson devices).

📋 2. Prerequisites

Before installing DeepStream, ensure your system meets the following requirements:

🔹 Hardware

NVIDIA discrete GPU (Pascal, Turing, Ampere, Ada, or later)

Minimum 6 GB VRAM recommended

At least 16 GB system RAM

Sufficient storage (~10 GB free)

🔹 Software

Ubuntu 20.04 LTS or Ubuntu 22.04 LTS (recommended)

CUDA Toolkit (matching version required by DeepStream)

TensorRT (matching version required by DeepStream)

cuDNN (matching version required by TensorRT/CUDA)

NVIDIA GPU Driver (must match CUDA version)

⚠️ Important: Each version of DeepStream requires specific versions of CUDA, TensorRT, cuDNN, and drivers. This was one of the major challenges I faced while setting up DeepStream.

👉 To check the correct compatibility for your setup, refer to NVIDIA’s official DeepStream SDK Compatibility Matrix
.

🔧 3. Installation Steps (from Scratch)
Step 1: Install NVIDIA GPU Driver

`bash`
```
sudo apt update
sudo ubuntu-drivers autoinstall
sudo reboot
```

Verify installation:

`bash`
```
nvidia-smi
```

Step 2: Install CUDA

Download CUDA Toolkit from [CUDA Downloads](https://developer.nvidia.com/cuda-downloads)


Install using .deb (local) package. Example:

`bash`
```
sudo dpkg -i cuda-repo-<version>_amd64.deb
sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/7fa2af80.pub
sudo apt update
sudo apt install -y cuda
```

Add CUDA to path:

`bash`
```
echo 'export PATH=/usr/local/cuda/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```

Step 3: Install cuDNN

Download matching cuDNN from NVIDIA cuDNN
.

Extract and copy files:

`bash`
```
tar -xvzf cudnn-linux-x86_64-<version>.tgz
sudo cp cuda/include/* /usr/local/cuda/include/
sudo cp cuda/lib64/* /usr/local/cuda/lib64/
```

Step 4: Install TensorRT

Download TensorRT .deb files from NVIDIA TensorRT
.

Install:

`bash`
```
sudo dpkg -i nv-tensorrt-local-repo-ubuntu2004-<version>_amd64.deb
sudo apt update
sudo apt install tensorrt
```

Step 5: Install DeepStream SDK

Download DeepStream .deb package from NVIDIA DeepStream Downloads
.

Install:

`bash`
```
sudo apt install ./deepstream-<version>_amd64.deb
```

Verify installation:

`bash`
```
deepstream-app --version-all
```

🧪 4. Running DeepStream Samples

DeepStream provides sample apps to test your setup.

Clone my GitHub repository for a detailed walkthrough on running DeepStream samples:
👉 Siddh01110/DeepStream-Samples

Example test:

`bash`
```
cd /opt/nvidia/deepstream/deepstream/sources/apps/sample_apps/deepstream-test1
sudo ./deepstream-test1-app <sample_h264_video.mp4>
```

✅ 5. Verification

After installation, run:

`bash`
```
nvidia-smi
deepstream-app --version-all
```

You should see details of CUDA, TensorRT, cuDNN, and DeepStream.

🔗 References

DeepStream SDK Official Page

DeepStream SDK Release Notes (Compatibility Matrix)

CUDA Toolkit Downloads

cuDNN Downloads

TensorRT Downloads
