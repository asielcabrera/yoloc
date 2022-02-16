# Installing The Base System
```
git clone https://github.com/asielcabrera/yoloc.git
cd yoloc
make
```


# Compiling With CUDA

Once you have CUDA installed, change the first line of the Makefile in the base directory to read:

```
GPU=1
```

# Compiling With OpenCV

First install OpenCV. If you do this from source it will be long and complex so try to get a package manager to do it for you.

Next, change the 2nd line of the Makefile to read:

```
OPENCV=1
```

# Detection Using A Pre-Trained Model

You already have the config file for YOLO in the cfg/ subdirectory. You will have to download the pre-trained weight file here (237 MB). Or just run this:

```
wget https://pjreddie.com/media/files/yolov3.weights
```
Then run the detector!

```
./darknet detect cfg/yolov3.cfg yolov3.weights data/dog.jpg
```

You will see some output like this:

```
layer     filters    size              input                output
    0 conv     32  3 x 3 / 1   416 x 416 x   3   ->   416 x 416 x  32  0.299 BFLOPs
    1 conv     64  3 x 3 / 2   416 x 416 x  32   ->   208 x 208 x  64  1.595 BFLOPs
    .......
  105 conv    255  1 x 1 / 1    52 x  52 x 256   ->    52 x  52 x 255  0.353 BFLOPs
  106 detection
truth_thresh: Using default '1.000000'
Loading weights from yolov3.weights...Done!
data/dog.jpg: Predicted in 0.029329 seconds.
dog: 99%
truck: 93%
bicycle: 99%
```


# Real-Time Detection on a Webcam

Running YOLO on test data isn't very interesting if you can't see the result. Instead of running it on a bunch of images let's run it on the input from a webcam!

To run this demo you will need to compile yoloc with `CUDA and OpenCV`. Then run the command:

```
./darknet detector demo cfg/coco.data cfg/yolov3.cfg yolov3.weights
```

YOLO will display the current FPS and predicted classes as well as the image with bounding boxes drawn on top of it.

You will need a webcam connected to the computer that OpenCV can connect to or it won't work. If you have multiple webcams connected and want to select which one to use you can pass the flag `-c <num>` to pick (OpenCV uses webcam 0 by default).

You can also run it on a video file if OpenCV can read the video:

```
./darknet detector demo cfg/coco.data cfg/yolov3.cfg yolov3.weights <video file>
```

# Train models