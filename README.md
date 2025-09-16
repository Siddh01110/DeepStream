# DeepStream-Samples

#Check all the dependencies versions

`bash`
```
deepstream-app --version-all
```
You will see something like:

```
(pr_venv) sidd@sidd:/opt/nvidia/deepstream/deepstream-7.1/sources/apps/sample_apps/deepstream-test1$ deepstream-app --version-all
deepstream-app version 7.1.0
DeepStreamSDK 7.1.0
CUDA Driver Version: 12.9
CUDA Runtime Version: 12.2
TensorRT Version: 10.2
cuDNN Version: 0.6
libNVWarp360 Version: 2.0.1d3
```

#Locate Samples files

`bash`
```
/opt/nvidia/deepstream/deepstream-7.1/sources/
```
You will see something like:

```
(pr_venv) sidd@sidd:~/Documents/Projects/industry_cv_safety$ /opt/nvidia/deepstream/deepstream-7.1/sources/
bash: /opt/nvidia/deepstream/deepstream-7.1/sources/: Is a directory
```

#List all the files present in it

`bash`
```
cd ~/Documents/Projects/industry_cv_safety
ls -l /opt/nvidia/deepstream/deepstream-7.1/sources/
```

You will see something like:

```
total 36
drwxr-xr-x  5 root root 4096 Jul 19 17:47 apps
drwxr-xr-x 23 root root 4096 Jul 19 17:47 gst-plugins
drwxr-xr-x  4 root root 4096 Jul 19 17:47 includes
drwxr-xr-x 19 root root 4096 Jul 19 17:47 libs
drwxr-xr-x  3 root root 4096 Jul 19 17:47 SONYCAudioClassifier
drwxr-xr-x  3 root root 4096 Jul 19 17:47 tools
drwxr-xr-x  2 root root 4096 Jul 19 17:47 tracker_ReID
drwxr-xr-x  5 root root 4096 Jul 19 17:47 TritonBackendEnsemble
drwxr-xr-x  4 root root 4096 Jul 19 17:47 TritonOnnxYolo
```

#list Samples

`bash`
```
cd /opt/nvidia/deepstream/deepstream-7.1/sources/apps/sample_apps
ls
```
You will see something like:

```
deepstream-3d-action-recognition   deepstream-audio                deepstream-image-meta-test         deepstream-nvdsanalytics-test  deepstream-test1   deepstream-transfer-learning-app
deepstream-3d-depth-camera         deepstream-avsync               deepstream-infer-tensor-meta-test  deepstream-nvof-test           deepstream-test2   deepstream-ucx-test
deepstream-3d-lidar-sensor-fusion  deepstream-can-orientation-app  deepstream-lidar-inference-app     deepstream-opencv-test         deepstream-test3   deepstream-user-metadata-test
deepstream-app                     deepstream-dewarper-test        deepstream-mrcnn-test              deepstream-preprocess-test     deepstream-test4
deepstream-appsrc-cuda-test        deepstream-gst-metadata-test    deepstream-multigpu-nvlink-test    deepstream-segmentation-test   deepstream-test5
deepstream-appsrc-test             deepstream-image-decode-test    deepstream-nmos                    deepstream-server              deepstream-testsr
```

#Build file using **sudo make**

`bash`
```
cd /opt/nvidia/deepstream/deepstream-7.1/sources/apps/sample_apps/deepstream-test1
sudo make
```

#Error would be 
You will see something like:

```
Makefile:15: *** "CUDA_VER is not set".  Stop.
```

#build correctly through

`bash`
```
sudo make CUDA_VER=12.2 #12.x
```
You will see something like:

```
(pr_venv) sidd@sidd:/opt/nvidia/deepstream/deepstream-7.1/sources/apps/sample_apps/deepstream-test1$ sudo make CUDA_VER=12.2

cc -c -o deepstream_test1_app.o -I../../../includes -I /usr/local/cuda-12.2/include -pthread -I/usr/include/gstreamer-1.0 -I/usr/include/x86_64-linux-gnu -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include deepstream_test1_app.c
cc -o deepstream-test1-app deepstream_test1_app.o -lgstreamer-1.0 -lgobject-2.0 -lglib-2.0 -L/usr/local/cuda-12.2/lib64/ -lcudart -L/opt/nvidia/deepstream/deepstream-7.1/lib/ -lnvdsgst_meta -lnvds_meta -lnvds_yml_parser -lnvdsgst_helper -lcuda -Wl,-rpath,/opt/nvidia/deepstream/deepstream-7.1/lib/ 
```

#Check the video sample stored locally

`bash`
```
ls -l /opt/nvidia/deepstream/deepstream-7.1/samples/streams/sample_720p.h264
```

You will see something like:

```
-rw-r--r-- 1 root root 14759548 Apr 25  2023 /opt/nvidia/deepstream/deepstream-7.1/samples/streams/sample_720p.h264
```

#Run the deepstream sample

`bash`
```
./deepstream-test1-app /opt/nvidia/deepstream/deepstream-7.1/samples/streams/sample_720p.h264
```
