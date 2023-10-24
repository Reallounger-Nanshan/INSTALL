# 安装OpenCV

## (一) 下载源文件

### 1. OpenCV

* 下载链接：https://github.com/opencv/opencv/releases


### 2. OpenCV_contrib

* 下载链接：[Tags · opencv/opencv_contrib · GitHub](https://github.com/opencv/opencv_contrib/tags)



## (二) 预处理

### 1. cmake和依赖库

* ```
  sudo apt-get install cmake


* ```
  sudo apt-get install build-essential libgtk2.0-dev libgtk-3-dev libavcodec-dev libavformat-dev libjpeg-dev libswscale-dev libtiff5-dev libgtk2.0-dev pkg-config

### 2. 支持Python的依赖

* ```
  sudo apt install python3-dev python3-numpy
  ```

* ```
  # streamer支持
  sudo apt install libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev

* ```
  # 可选的依赖
  sudo apt install libpng-dev libopenexr-dev libtiff-dev libwebp-dev
  ```


### 3. ippicv

* ```cmake
  # 在文件~/opencv4.5.3/3rdparty/ippicv/ippicv.cmake 中找3个关键值
  
  # 43行：下载目录的地址https://raw.githubusercontent.com/opencv/opencv_3rdparty/${IPPICV_COMMIT}/ippicv/
  # 5行：IPPICV_COMMIT值：a56b6ac6f030c312b2dce17430eef13aed9af274
  # 10行：文件名：ippicv_2020_lnx_intel64_20191018_general.tgz
  # 三个值组合起来，就是下载地址https://raw.githubusercontent.com/opencv/opencv_3rdparty/a56b6ac6f030c312b2dce17430eef13aed9af274/ippicv/ippicv_2020_lnx_intel64_20191018_general.tgz
  ```
  
* ```cmake
  # 下载好了之后，直接放到~/OpenCV4.5.3/opencv-4.5.3/3rdparty/ippicv/目录下，修改 ippicv.cmake文件的第42行
  
  # "https://raw.githubusercontent.com/opencv/opencv_3rdparty/${IPPICV_COMMIT}/ippicv/"
  "file:///home/reallounger/OpenCV4.5.3/opencv-4.5.3/3rdparty/ippicv/"

### 4. boostdesc_bgm.i

* ```
  # 预先下载文件
  
  boostdesc_bgm.i
  boostdesc_bgm_bi.i
  boostdesc_bgm_hd.i
  boostdesc_lbgm.i
  boostdesc_binboost_064.i
  boostdesc_binboost_128.i
  boostdesc_binboost_256.i
  ```

* ```cmake
  # 把这7个文件拷贝到 ~/OpenCV4.5.3/opencv_contrib-4.5.3/modules/xfeatures2d/src/文件夹下
  # 修改 ~/OpenCV4.5.3/opencv_contrib-4.5.3/modules/xfeatures2d/cmake/download_boostdesc.cmake 文件
  
  # 12-18行
  set(hash_BGM "c9fc90c2bc8c4bc4bf7700356a767a43")
  set(hash_BGM_BI "2b0afab9d0eb6fbffc355a222161834f")
  set(hash_BGM_HD "5a1b2a4e9afcb38ca3851f4df43c5aea")
  set(hash_BINBOOST_064 "f6cd2116631666e2f719ead236b82f53")
  set(hash_BINBOOST_128 "81275c26e6c6d559b9ecefc95319e570")
  set(hash_BINBOOST_256 "5fff6b5824b5e59ceb153f02576cf1e2")
  set(hash_LBGM "bf7e3c0acd53bf4cccfb9c02a0a46b69")
  
  # 27行
  # "https://raw.githubusercontent.com/opencv/opencv_3rdparty/${OPENCV_3RDPARTY_COMMIT}/"
  "file:///home/reallounger/OpenCV4.5.3/opencv_contrib-4.5.3/modules/xfeatures2d/src"

### 5. vgg

* ```
  # 预先下载文件
  
  vgg_generated_120.i
  vgg_generated_64.i
  vgg_generated_80.i
  vgg_generated_48.i
  ```

* ```cmake
  # 把这4个文件拷贝到 ~/OpenCV4.5.3/opencv_contrib-4.5.3/modules/xfeatures2d/src/文件夹下
  # 修改 ~/OpenCV4.5.3/opencv_contrib-4.5.3/modules/xfeatures2d/cmake/download_vgg.cmake 文件21行
  
   # "https://raw.githubusercontent.com/opencv/opencv_3rdparty/${OPENCV_3RDPARTY_COMMIT}/"
  "file:///home/reallounger/OpenCV4.5.3/opencv_contrib-4.5.3/modules/xfeatures2d/src/"

### 6. face_landmark_model.dat.zip

* 下载地址：https://raw.githubusercontent.com/opencv/opencv_3rdparty/8afa57abc8229d611c4937165d20e2a2d9fc5a12/face_landmark_model.dat
* 下载完成后解压为dat文件
* 暂时忽略
* 当cmake结束后，查看build/CMakeDownloadLog.txt
* 将文件拷贝至对应的目录：build/share/opencv4/testdata/cv/face



## (三) 编译

### 1. 创建build文件夹

* ```
  mkdir build && cd build
  ```

### 2. cmake

* ```
  cmake -D CMAKE_BUILD_TYPE=RELEASE \
        -D CMAKE_INSTALL_PREFIX=~/OpenCV4.5.3/opencv4.5.3_install/ \ #安装路径和编译路径不是同一个，如果同一个在后边make install 会报错
        # 自定义Python路径
        -D PYTHON3_INCLUDE_DIR=~/anaconda3/envs/python3.7/include/python3.7m \
        -D PYTHON3_LIBRARY=~/anaconda3/envs/python3.7/lib/libpython3.7m.so \
        -D PYTHON3_NUMPY_INCLUDE_DIRS=~/anaconda3/envs/python3.7/lib/python3.7/site-packages/numpy/core/include \
        -D PYTHON3_PACKAGES_PATH=~/anaconda3/envs/python3.7/lib/python3.7/site-packages
        -D WITH_CUDA=ON \ #使用CUDA
        -D WITH_CUBLAS=ON \
  	  -D CUDA_ARCH_BIN='7.0' \ #指定GPU算力，在NVIDIA官网查询
        -D OPENCV_EXTRA_MODULES_PATH=~/OpenCV4.5.3/opencv_contrib-4.5.3/modules \ #opencv_contrib modules路径
        -D OPENCV_GENERATE_PKGCONFIG=YES \
        -D BUILD_TIFF=ON \ # 防止编译出错
        ..
  ```
  
* ```
  # 可直接写成
  cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=~/OpenCV4.5.3/opencv4.5.3_install -D PYTHON3_INCLUDE_DIR=~/anaconda3/envs/python3.7/include/python3.7m -D PYTHON3_LIBRARY=~/anaconda3/envs/python3.7/lib/libpython3.7m.so -D PYTHON3_NUMPY_INCLUDE_DIRS=~/anaconda3/envs/python3.7/lib/python3.7/site-packages/numpy/core/include -D PYTHON3_PACKAGES_PATH=~/anaconda3/envs/python3.7/lib/python3.7/site-packages -D WITH_CUDA=ON -D WITH_CUBLAS=ON -D CUDA_ARCH_BIN='7.0' -D OPENCV_EXTRA_MODULES_PATH=~/OpenCV4.5.3/opencv_contrib-4.5.3/modules -D OPENCV_GENERATE_PKGCONFIG=YES -D BUILD_TIFF=ON ..
  ```
  
* 出现下图所示则表示cmake成功，否则应处理相应的报错再继续make

* ![cmake](../../Pictures/OpenCV/cmake.png)

* 如果cmake失败，需删除build文件夹重新cmake

* ```
  cd .. && rm -rf build

* 如果出现报错：runtime library [libtiff.so.5] in /usr/lib/x86_64-linux-gnu may be hidden by files in:  /usr/local/lib

  * ```
    rm /usr/local/lib/libtiff.so.5
    ```

  * ```
    sudo ln -s /usr/lib/x86_64-linux-gnu/libtiff.so.5 /usr/local/lib/libtiff.so.5

### 3. make

* ```
  # 查看CPU核心数
  nproc
  ```

* ```
  # 根据CPU核心数确定编译线程数
  make -j12
  ```
  
* 如果出现报错：/libgio-2.0.so.0：对‘g_atomic_ref_count_inc’未定义的引用

  * ```
    # 查看Anaconda和系统的libgio版本
    locate libgio-2.0.so.0
    ```

  * ```
    # Anaconda安装与系统的版本相同的版本
    conda install glib=2.56.1

* 如果出现报错：/lib/libpangoft2-1.0.so.0: undefined symbol

  * ```
    # 找到缺失的文件
    locate libpangoft2-1.0.so.0
    ```

  * ```
    # 将找到的文件拷贝到报错的目录之下
    cp /usr/lib/x86_64-linux-gnu/libpangoft2-1.0.so.0 ~/anaconda3/envs/python3.7/lib/libpangoft2-1.0.so.0
    ```

* 如果出现报错：undefined reference to `uuid_unparse_lower@UUID_1.0

  * ```
    # 查找Anaconda下冲突文件地址
    locate libuuid

  * ```
    cd target_dir && mkdir libuuid_bk
    ```

  * ```
    mv libuuid* libuuid_bk/
    ```

  * ```
    # 刷新数据库
    sudo updatedb

* 如果出现报错：/usr/local/lib/libcurl.so.4: no version information available (required by /usr/bin/curl)

  * ```
    rm -rf /usr/local/lib/libcurl.so.4
    ```

  * ```
    # 不知道目标文件在哪可以通过locate命令查找
    ln -s /usr/lib/x86_64-linux-gnu/libcurl.so.4.3.0 /usr/local/lib/libcurl.so.4

* ```
  # 如果编译失败
  make clean && make -j12
  
* ```
  # 若无问题则进行安装
  make install



## (四) 环境配置

### 1. 动态库配置

* ```
  # 进入root权限，否则无权限操作
  su

* ```
  echo "~/OpenCV4.5.3/opencv4.5.3_install/lib/"  >> /etc/ld.so.conf.d/opencv4.5.conf
  ```

* 如果出现报错：/usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_ops_train.so.8 不是符号链接

  * ```
    sudo ln -sf /usr/local/cuda-10.2/lib64/libcudnn.so.8.2.2 /usr/local/cuda-10.2/lib64/libcudnn.so.8
    ```

  * ```
    sudo ln -sf /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.8.2.2 /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.8
    ```

  * ```
    sudo ln -sf /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_adv_infer.so.8.2.2 /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_adv_infer.so.8
    ```

  * ```
    sudo ln -sf /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_adv_train.so.8.2.2 /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_adv_train.so.8
    ```

  * ```
    sudo ln -sf /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_ops_train.so.8.2.2 /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_ops_train.so.8
    ```

  * ```
    sudo ln -sf /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8.2.2 /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8
    ```

  * ```
    sudo ln -sf /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8.2.2 /usr/local/cuda-10.2/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8
    ```

* ```
  ldconfig



## (五) 配置OpenCV的PKG-CONFIG环境

### 1. 修改opencv4.pc文件

* ```
  sudo find / -iname opencv4.pc
  ```

* 如果上述配置成功，则opencv4.5.3_install/lib文件夹下会存在pkgconfig/opencv4.pc文件

* ```
  # 若无pkgconfig/opencv4.pc文件，则自行创建文件并复制粘贴以下内容
  
  # Package Information for pkg-config
  
  prefix=/home/reallounger/OpenCV4.5.3/opencv4.5.3_install
  exec_prefix=${prefix}
  libdir=${exec_prefix}/lib
  includedir=${prefix}/include/opencv4
  
  Name: OpenCV
  Description: Open Source Computer Vision Library
  Version: 4.5.3
  Libs: -L${exec_prefix}/lib -lopencv_gapi -lopencv_stitching -lopencv_aruco -lopencv_barcode -lopencv_bgsegm -lopencv_bioinspired -lopencv_ccalib -lopencv_cudabgsegm -lopencv_cudafeatures2d -lopencv_cudaobjdetect -lopencv_cudastereo -lopencv_dnn_objdetect -lopencv_dnn_superres -lopencv_dpm -lopencv_face -lopencv_freetype -lopencv_fuzzy -lopencv_hfs -lopencv_img_hash -lopencv_intensity_transform -lopencv_line_descriptor -lopencv_mcc -lopencv_quality -lopencv_rapid -lopencv_reg -lopencv_rgbd -lopencv_saliency -lopencv_stereo -lopencv_structured_light -lopencv_phase_unwrapping -lopencv_superres -lopencv_cudacodec -lopencv_surface_matching -lopencv_tracking -lopencv_highgui -lopencv_datasets -lopencv_text -lopencv_plot -lopencv_videostab -lopencv_cudaoptflow -lopencv_optflow -lopencv_cudalegacy -lopencv_videoio -lopencv_cudawarping -lopencv_wechat_qrcode -lopencv_xfeatures2d -lopencv_shape -lopencv_ml -lopencv_ximgproc -lopencv_video -lopencv_dnn -lopencv_xobjdetect -lopencv_objdetect -lopencv_calib3d -lopencv_imgcodecs -lopencv_features2d -lopencv_flann -lopencv_xphoto -lopencv_photo -lopencv_cudaimgproc -lopencv_cudafilters -lopencv_imgproc -lopencv_cudaarithm -lopencv_core -lopencv_cudev
  Libs.private: -lm -lpthread -lcudart_static -ldl -lrt -lnppc -lnppial -lnppicc -lnppicom -lnppidei -lnppif -lnppig -lnppim -lnppist -lnppisu -lnppitc -lnpps -lcublas -lcufft -L-L/usr/local/cuda-10.2 -llib64 -L-L/usr/lib -lx86_64-linux-gnu
  Cflags: -I${includedir}

### 2. 配置环境

* ```
  sudo gedit ~/.bashrc

* ```bash
  # 在文件底部加上
  PKG_CONFIG_PATH=$PKG_CONFIG_PATH:~/OpenCV4.5.3/opencv4.5.3_install/lib/pkgconfig
  export PKG_CONFIG_PATH
  ```

* ```
  source ~/.bashrc

### 3. 验证是否配置成功

* ```
  pkg-config --libs opencv4
  ```

* 如下图所示则配置成功
* ![pkg-config--libs_opencv4](../../Pictures/OpenCV/pkg-config--libs_opencv4.png)



## (六) Python-OpenCV环境

### 1. 查找opencv库

* ```
  sudo find / -iname cv2*.so
  ```

### 2. 链接

* ```
  # 链接到系统自带的Python3解释器中
  sudo ln -s /usr/local/opencv4.4/opencv4/build/lib/python3.6/dist-packages/cv2/python-3.6/cv2.cpython-36m-x86_64-linux-gnu.so /usr/lib/python3/dist-packages/cv2.so
  ```

* ```
  链接到Anaconda创建的虚拟环境Python3解释器中
  ln -s /usr/local/opencv4.4/opencv4/build/lib/python3.6/dist-packages/cv2/python-3.6/cv2.cpython-36m-x86_64-linux-gnu.so ~/anaconda3/lib/python3.7/site-packages/cv2.so
  ```



## (七) 测试OpenCV

### 1. 测试c++下OpenCV

#### 1.1 打开相机

* ```
  cd xx/opencv-4.5.3/samples/cpp/example_cmake/
  ```

* ```
  g++ -std=c++11 example.cpp `pkg-config --libs --cflags opencv4` -o example
  ```

* 如果出现报错：libopencv_imgcodecs.so: 对`TIFFReadDirectory@LIBTIFF_4.0未定义的引用

  * ```
    # 查找目标库依赖，xx为目标所在完整路径
    ldd /xx/libopencv_imgcodecs.so | grep tiff
    ```

  * 若二次报错：依赖文件未定义，则采取3.3方式解决

  * 若无报错，直接看依赖所在路径，如下图所示（图为网络图片，具体路径以实际查到的路径为准）

  * 若无报错，直接看依赖所在路径，如下图所示（图为网络图片，具体路径以实际查到的路径为准）

  * ![ldd_tiff](./Pictures/OpenCV/ldd_tiff.png)

  * ```
    # 将查找到的路径加入CMakelist.txt中
    
    target_link_libraries(opencv_example PRIVATE
    ${OpenCV_LIBS}
    ~/anaconda3/lib/libtiff.so.5
    )

* ```
  ./example
  ```

* 如果出现报错：./example: libopencv_highgui.so.3.4: cannot open shared object file

  * ```
    # 查找相关依赖
    ldd ./example
    ```

  * ```
    # 显示not found的则是未找到的库
    # locate命令查找相关库所在路径
    ```

  * ```
    # 配置环境变量
    sudo gedit /etc/ld.so.conf.d/opencv.conf
    
    # 写入查询到的路径

  * ```
    sudo ldconfig

* 出现如图所示情况即安装成功

* ![example](./Pictures/OpenCV/example.png)

#### 1.2 打开图片

* ```
  gedit test.cpp
  ```

* ```c++
  #include "opencv2/core.hpp"
  #include "opencv2/imgproc.hpp"
  #include "opencv2/highgui.hpp"
  #include <iostream>
  
  using namespace cv;
  using namespace std;
  
  int main(int artc, char** argv){
      Mat src = imread("/home/xxx/test.png", IMREAD_GRAYSCALE);
  
      if (src.empty()) {
          printf("could not load image...\n");
          return -1;
      }
      namedWindow("input", WINDOW_AUTOSIZE);
      imshow("input", src);
  
      waitKey(0);
      return 0;
  }
  ```

* ```
  g++ -std=c++11 test.cpp `pkg-config --libs --cflags opencv4` -o test
  ```

* ```
  ./test

### 2. 测试Python-OpenCV

* ```
  python3
  ```

* ```
  import cv2

* 如果出现报错：ImportError: /home/reallounger/anaconda3/bin/../lib/libpangoft2-1.0.so.0: undefined symbol: FcWeightFromOpenTypeDouble
  
  * 错误原因：Anaconda的 lib 把 Python需要的各种 lib 单独列出，造成其与系统中的库版本不一致
  
  * ```
    sudo updatedb
    locate libpangoft2-1.0.so.0
    ```
  
  * ```
    cd /home/reallounger/anaconda3/bin/../lib/
    rm libpangoft2-1.0.so.0
    cp /usr/lib/x86_64-linux-gnu/libpangoft2-1.0.so.0 libpangoft2-1.0.so.0
  
* ```
  cv2.__version__
  ```

## (八) 卸载

### 1. 卸载

* ```
  cd build

* ```
  sudo make uninstall
  ```

* ```
  cd ..
  ```

* ```
  sudo rm -rf build
  ```

* ```
  sudo rm -rf /etc/ld.so.conf.d/opencv4.5.conf

* ```
  sudo gedit ~/.bashrc
  ```

* ```bash
  # 删除
  PKG_CONFIG_PATH=$PKG_CONFIG_PATH:~/OpenCV4.5.3/opencv4.5.3_install/lib/pkgconfig
  export PKG_CONFIG_PATH
  ```

* ```
  source ~/.bashrc
  ```
