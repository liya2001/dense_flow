Extracting dense flow field given a video.

#### Depencies:
- LibZip: 
to install on ubuntu ```apt-get install libzip-dev``` on mac ```brew install libzip```

#### For OpenCV 3 Users
Please see the [opencv-3.1](https://github.com/yjxiong/dense_flow/tree/opencv-3.1) branch. Many thanks to @victorhcm for the contributions!

### Install
```
git clone --recursive http://github.com/yjxiong/dense_flow
```

### Build
```markdown
mkdir build && cd build
# `export OpenCV_DIR=/path/to/opencv-2.4.13/release/` if necessary
cmake -D CUDA_USE_STATIC_CUDA_RUNTIME=OFF .. 
make -j
```
##### Build problems and solutions
1. Error: cannot find -lopencv_dep_cudart  
   [solution](https://github.com/yjxiong/temporal-segment-networks/issues/54): add -D CUDA_USE_STATIC_CUDA_RUNTIME=OFF when build dense_flow 
    ```markdown
    cmake -D CUDA_USE_STATIC_CUDA_RUNTIME=OFF ..
    ``` 
    
2. Can't find right OpenCV version and path  
    Export opencv build path(containing the OpenCVConfig.cmake file) before `make ..`
    ```markdown
    export OpenCV_DIR=/path/to/opencv-2.4.13/release/
    ```
    [reference](https://stackoverflow.com/questions/8711109/could-not-find-module-findopencv-cmake-error-in-configuration-process)
   
3. Boost python version problem  
    ImportError: /dense_flow/build/libpydenseflow.so: undefined symbol: _ZN5boost6python6detail11init_moduleER11PyModuleDefPFvvE   
    [Solution](https://github.com/yjxiong/anet2016-cuhk/issues/11): Modify line 23 of CMakeLists.txt to FIND_PACKAGE(PythonLibs 2.7 REQUIRED). The default one is for Python3.5m
### Usage
```
./extract_gpu -f test.avi -x tmp/flow_x -y tmp/flow_y -i tmp/image -b 20 -t 1 -d 0 -s 1 -o dir
```
- `test.avi`: input video
- `tmp`: folder containing RGB images and optical flow images
- `dir`: output generated images to folder. if set to `zip`, will write images to zip files instead.

### Warp Flow
The warp optical flow is used in the following paper

```
@inproceedings{TSN2016ECCV,
  author    = {Limin Wang and
               Yuanjun Xiong and
               Zhe Wang and
               Yu Qiao and
               Dahua Lin and
               Xiaoou Tang and
               Luc {Van Gool}},
  title     = {Temporal Segment Networks: Towards Good Practices for Deep Action Recognition},
  booktitle   = {ECCV},
  year      = {2016},
}
```

To extract warp flow, use the command
```
./extract_warp_gpu -f test.avi -x tmp/flow_x -y tmp/flow_y -i tmp/image -b 20 -t 1 -d 0 -s 1 -o dir
```
