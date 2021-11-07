# YoloV3 - DarkNet

<p align="center">
  <img src="https://camo.githubusercontent.com/6b3c6c1109586f5f3ddf8967fa4eaf787c7b45fe3df6d89111d6f9c7c1045769/687474703a2f2f706a7265646469652e636f6d2f6d656469612f66696c65732f6461726b6e65742d626c61636b2d736d616c6c2e706e67" width=250/><br/><br/>
</p>



## Installation (on Windows)

### How to add a path to the $PATH environment variable

Check this tutorial first if you don't know how to add new codes to the global path 😄

https://docs.microsoft.com/en-us/previous-versions/office/developer/sharepoint-2010/ee537574(v=office.14)

### CUDA

Go on this link and install it with the given executable.

https://developer.nvidia.com/cuda-10.0-download-archive

> :warning: **Make sure to select an appropriate installation folder, we'll add it to the $PATH environment variable.**

Paths to add:

```
...\NVIDIA GPU Computing Toolkit\CUDA\v11.4\bin
```

```
...\NVIDIA GPU Computing Toolkit\CUDA\v11.4\libnvvp
```

### OpenCV

Go on this link and install it with the given executable.

https://sourceforge.net/projects/opencvlibrary/files/opencv-win/3.3.0/

> :warning: **Make sure to select an appropriate installation folder, we'll add it to the $PATH environment variable.**

Paths to add:

```
...\OpenCV\build\include
```

```
...\OpenCV\build\x64\vc14\lib
```

### CUDNN

https://developer.nvidia.com/rdp/cudnn-archive

### DarkNet

Pull the `darknet` submodule of this project by launching this command (at the root of the project):
```
git submodule update --init darknet
```

Then open the Visaul Studio Solution in `.\darknet\build\darknet\darknet.sln`.

From now on, you'll have to build `darknet`.

Like so:



## How to use it

## Our configuration

https://developer.nvidia.com/rdp/cudnn-archive
