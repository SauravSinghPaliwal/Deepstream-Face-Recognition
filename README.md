# Deepstream-Face-Recognition
Face Recogination using Retinaface and Arcface in Deepstream.

## Requirements
Deepstream-6.1 docker container

[nvcr.io/nvidia/deepstream:6.1-devel](nvcr.io/nvidia/deepstream:6.1-devel)

docker run --gpus all -it -d --privileged -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY -v {your local directory which is to be mounted}:{mount directory} -p 8554:8554  nvcr.io/nvidia/deepstream:6.1-devel

## Pretrained Models

### For Arcface 
1) Downloads the the Arcface model weights and images from the link given below

   [https://drive.google.com/drive/folders/1oolpyEBaStNk3IKjYtlApO_QKMCs6NMy?usp=sharing](https://drive.google.com/drive/folders/1oolpyEBaStNk3IKjYtlApO_QKMCs6NMy?usp=sharing)

2) git clone https://github.com/wang-xinyu/tensorrtx.git
3) cd tensorrtx/arcface
4) Copy the arcface weights to tensorrtx/arcface
5) Copyjoey0.ppm and joey1.ppm to tensorrtx/arcface
6) mkdir build && cd build && cmake .. && make
7) ./arcface-r100 -s (This will generate arcface-r100.engine file)
8) mkdir /opt/tensorrt
9) copy arcface-r100.engine to /opt/tensorrt/

### For Retinaface
1) Downloads the the Arcface model weights and images from the link given below

   [https://drive.google.com/drive/folders/1otvST_oTLGMua7TUGsx1n6aZ5xNXHbhK?usp=sharing](https://drive.google.com/drive/folders/1otvST_oTLGMua7TUGsx1n6aZ5xNXHbhK?usp=sharing)
   
3) cd tensorrtx/retinaface
4) Copy the Retinaface weights to tensorrtx/retinaface
5) vim retina_r50.cpp
6) Change the batch size and Precision according to yourself (* For me I set the batchsize as 32 according to my model and Precision as FP16) 
7) mkdir build && cd build && cmake .. && make
8) ./retina_r50 -s (This will generate retina_r50.engine file)
9) mkdir /opt/tensorrt
10) copy retina_r50.engine to /opt/tensorrt/
11) Also copy the libdecodeplugin.so to /opt/tensorrt

## Run the demo 

1) git clone https://github.com/SauravSinghPaliwal/Deepstream-Face-Recognition.git
2) cd Deepstream-Face-Recognition
3) move models directory to /opt/
   #### Copy the engine files to their respective directory
4) cp /opt/tensorrt/arcface-r100.engine  /opt/models/arcface/
5) cp /opt/tensorrt/retina_r50.engine  /opt/models/retinaface/
6) cp /opt/tensorrt/libdecodeplugin.so /opt/models/retinaface/
   #### Modify all path input video path in test/face_test.py
   #### Start kafka service before everything.
7) wget https://archive.apache.org/dist/kafka/3.4.0/kafka_2.13-3.4.0.tgz
8) tar -xzf kafka_2.13-3.4.0.tgz
9) cd kafka_2.13-3.4.0
10) bin/zookeeper-server-start.sh config/zookeeper.properties
11) bin/kafka-server-start.sh config/server.properties
    #### Now start the demo
12) python3 face_test_demo.py



