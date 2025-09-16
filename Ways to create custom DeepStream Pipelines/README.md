#  DeepStream Pipeline Creation & Execution with YOLO Models

This guide explains different ways to create and execute **DeepStream pipelines** for object detection using **YOLO models**.  
It covers **config-based execution, command-line pipelines, and custom applications**.

---

##  1. Prerequisites

- Installed **DeepStream SDK** (follow [installation guide](../README.md))  
- NVIDIA GPU with CUDA support  
- YOLO model converted to **TensorRT engine (`.engine`)** or **ONNX**  

 For YOLO → TensorRT conversion, you can use [DeepStream-YOLO](https://github.com/marcoslucianops/DeepStream-Yolo).

---

##  2. Ways to Create DeepStream Pipelines

There are **3 main approaches**:

1. **Config File Based (deepstream-app)**  
   - Define pipeline in `.txt` config file  
   - Run with `deepstream-app -c <config.txt>`  

2. **Command-Line GStreamer (`gst-launch-1.0`)**  
   - Build pipeline directly in CLI  
   - Useful for debugging and prototyping  

3. **Custom Applications (C/C++ or Python)**  
   - Use DeepStream SDK APIs  
   - Full flexibility (YOLO plugins, tracking, metadata, analytics)  

---

##  3. Method 1: Config File Based Pipeline

DeepStream comes with a sample config system (`/opt/nvidia/deepstream/deepstream-<version>/samples/configs/`).

### Example: YOLO + DeepStream Config

`source_yolo.txt`
```ini
[source0]
enable=1
type=3
uri=file://<PATH_TO_VIDEO>.mp4

[primary-gie]
enable=1
gpu-id=0
model-engine-file=../../models/yolo/yolov8.engine
batch-size=1
interval=0
gie-unique-id=1
network-type=0
process-mode=1
num-detected-classes=80
config-file=config_infer_primary_yolo.txt

[sink0]
enable=1
type=5
sync=0
```
Run:
`bash`
```
deepstream-app -c source_yolo.txt
```
---

## 4. Method 2: GStreamer Command-Line

DeepStream is based on GStreamer. You can launch pipelines directly via gst-launch-1.0.

Example: Simple Decode → YOLO Inference → OSD → Display

`bash`
```
gst-launch-1.0 filesrc location=<PATH_TO_VIDEO>.mp4 ! \
qtdemux ! h264parse ! nvv4l2decoder ! \
nvinfer config-file-path=config_infer_primary_yolo.txt ! \
nvdsosd ! nveglglessink sync=0
```

Example with RTSP Source:

`bash`
```
gst-launch-1.0 rtspsrc location=rtsp://<IP_CAM_URL> latency=200 ! \
rtph264depay ! h264parse ! nvv4l2decoder ! \
nvinfer config-file-path=config_infer_primary_yolo.txt ! \
nvdsosd ! nveglglessink sync=0
```
---

## 5. Method 3: Custom Applications (C++ / Python)

When you need more control (metadata, tracking, saving results), you can write apps using the DeepStream SDK.

Example: Python App with YOLO

deepstream_yolo.py

`python`
```
import pyds
import gi
gi.require_version('Gst', '1.0')
from gi.repository import Gst

# Initialize GStreamer
Gst.init(None)

# Create pipeline
pipeline = Gst.parse_launch(
    "filesrc location=sample.mp4 ! qtdemux ! h264parse ! nvv4l2decoder ! "
    "nvinfer config-file-path=config_infer_primary_yolo.txt ! "
    "nvdsosd ! nveglglessink"
)

# Run pipeline
pipeline.set_state(Gst.State.PLAYING)
bus = pipeline.get_bus()
msg = bus.timed_pop_filtered(
    Gst.CLOCK_TIME_NONE,
    Gst.MessageType.ERROR | Gst.MessageType.EOS
)
pipeline.set_state(Gst.State.NULL)
```

Run:

`bash`
```
python3 deepstream_yolo.py
```
---

## 6. Comparison of Methods
| Method          | Ease of Use | Flexibility | Best For                         |
|-----------------|-------------|-------------|----------------------------------|
| Config Files    |  Easy     |  Limited  | Quick testing, prototyping       |
| GStreamer CLI   |  Medium  |  Limited  | Debugging, one-off pipelines     |
| Custom Apps     |  Complex  |  Full     | Production apps, metadata handling |


---

## 7. Verification

Check DeepStream version:

`bash`
```
deepstream-app --version-all
```

Test YOLO config pipeline:

`bash`
```
deepstream-app -c source_yolo.txt
```

Test GStreamer pipeline:

`bash`
```
gst-launch-1.0 filesrc location=sample.mp4 ! qtdemux ! h264parse ! nvv4l2decoder ! \
nvinfer config-file-path=config_infer_primary_yolo.txt ! nvdsosd ! nveglglessink
```
---

## References

[DeepStream SDK Documentation](https://docs.nvidia.com/metropolis/deepstream/dev-guide/index.html)

[DeepStream Getting Started](https://developer.nvidia.com/deepstream-getting-started?utm_source=chatgpt.com)

[DeepStream-YOLO GitHub](https://github.com/marcoslucianops/DeepStream-Yolo)
