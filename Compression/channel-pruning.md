# channel-pruning 通道减枝

```
zhijinlin@ubuntu:~/channel-pruning$ python3 train.py -action c3 -caffe 0
no lighting pack
[libprotobuf WARNING google/protobuf/io/coded_stream.cc:604] Reading dangerously large protocol message.  If the message turns out to be larger than 2147483647 bytes, parsing will be halted for security reasons.  To increase the limit (or to disable these warnings), see CodedInputStream::SetTotalBytesLimit() in google/protobuf/io/coded_stream.h.
[libprotobuf WARNING google/protobuf/io/coded_stream.cc:81] The total number of bytes read was 553432081
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
stage0 freeze
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
temp/bn_vgg.prototxt
[libprotobuf WARNING google/protobuf/io/coded_stream.cc:604] Reading dangerously large protocol message.  If the message turns out to be larger than 2147483647 bytes, parsing will be halted for security reasons.  To increase the limit (or to disable these warnings), see CodedInputStream::SetTotalBytesLimit() in google/protobuf/io/coded_stream.h.
[libprotobuf WARNING google/protobuf/io/coded_stream.cc:81] The total number of bytes read was 553433062
including last conv layer!
run for 500 batches nFeatsPerBatch 100
Extracting conv1_1 (50000, 64)
Extracting conv1_2 (50000, 64)
Extracting conv2_1 (50000, 128)
Extracting conv2_2 (50000, 128)
Extracting conv3_1 (50000, 256)
Extracting conv3_2 (50000, 256)
Extracting conv3_3 (50000, 256)
Extracting conv4_1 (50000, 512)
Extracting conv4_2 (50000, 512)
Extracting conv4_3 (50000, 512)
Extracting conv5_1 (50000, 512)
Extracting conv5_2 (50000, 512)
Extracting conv5_3 (50000, 512)
Acc  88.180
wrote memory data layer to temp/mem_bn_vgg.prototxt
freezing imgs to temp/frozen500.pickle
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
stage1 speed3.0
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[libprotobuf WARNING google/protobuf/io/coded_stream.cc:604] Reading dangerously large protocol message.  If the message turns out to be larger than 2147483647 bytes, parsing will be halted for security reasons.  To increase the limit (or to disable these warnings), see CodedInputStream::SetTotalBytesLimit() in google/protobuf/io/coded_stream.h.
[libprotobuf WARNING google/protobuf/io/coded_stream.cc:81] The total number of bytes read was 553433062
loading imgs from temp/frozen500.pickle
loaded
Extracting X relu1_1 From Y conv1_2 stride 1
Acc  88.180
spatial_decomposition 848.4372959136963
run for 500 batches nFeatsPerBatch 100
Extracting conv1_2 (50000, 64)
Acc  88.200
Reconstruction Err 0.014325546255883519
channel_decomposition 106.49921560287476
Extracting X pool1 From Y conv2_1 stride 1
Acc  88.120
rMSE 0.015392468472299754
channel_pruning 132.2307732105255
Extracting X pool1 From Y conv2_1 stride 1
Acc  88.240
spatial_decomposition 1318.4993188381195
run for 500 batches nFeatsPerBatch 100
Extracting conv2_1 (50000, 128)
Acc  88.260
Reconstruction Err 0.02192186164728999
channel_decomposition 2545.6551835536957
Extracting X conv2_1 From Y conv2_2 stride 1
Acc  88.260
rMSE 0.022449152559490558
channel_pruning 4916.143956422806
Extracting X relu2_1 From Y conv2_2 stride 1
Acc  88.220
spatial_decomposition 7962.176743030548
run for 500 batches nFeatsPerBatch 100
Extracting conv2_2 (50000, 128)
Acc  88.060
Reconstruction Err 0.051780403666922804
channel_decomposition 1915.290130853653
Extracting X pool2 From Y conv3_1 stride 1
Acc  88.060
rMSE 0.04322967236122802
channel_pruning 156.1571490764618
Extracting X pool2 From Y conv3_1 stride 1
Acc  88.180
spatial_decomposition 6955.229420185089
run for 500 batches nFeatsPerBatch 100
Extracting conv3_1 (50000, 256)
Acc  88.020
Reconstruction Err 0.06430198799895964
channel_decomposition 2823.1375646591187
Extracting X conv3_1 From Y conv3_2 stride 1
Acc  88.100
rMSE 0.05362905075470961
channel_pruning 144.4986069202423
Extracting X relu3_1 From Y conv3_2 stride 1
Acc  88.140
spatial_decomposition 7657.579173564911
run for 500 batches nFeatsPerBatch 100
Extracting conv3_2 (50000, 256)
Acc  88.140
Reconstruction Err 0.0983451912838961
channel_decomposition 6367.251094341278
Extracting X conv3_2 From Y conv3_3 stride 1
Acc  88.080
rMSE 0.07908570446038755
channel_pruning 2399.825722694397
Extracting X relu3_2 From Y conv3_3 stride 1
Acc  88.140
spatial_decomposition 7967.679211616516
run for 500 batches nFeatsPerBatch 100
Extracting conv3_3 (50000, 256)
Acc  88.140
Reconstruction Err 0.11373156940781838
channel_decomposition 5676.430692911148
Extracting X pool3 From Y conv4_1 stride 1
Acc  88.180
spatial_decomposition 11609.11757349968
run for 500 batches nFeatsPerBatch 100
Extracting conv4_1 (50000, 512)
Acc  88.140
Reconstruction Err 0.10901618356050842
channel_decomposition 9018.517218112946
Extracting X conv4_1 From Y conv4_2 stride 1
Acc  88.240
rMSE 0.08151337935791692
channel_pruning 2330.285479784012
Extracting X relu4_1 From Y conv4_2 stride 1
Acc  88.280
spatial_decomposition 13770.419756889343
run for 500 batches nFeatsPerBatch 100
Extracting conv4_2 (50000, 512)
Acc  88.180
Reconstruction Err 0.16666235285048864
channel_decomposition 8392.841899633408
Extracting X conv4_2 From Y conv4_3 stride 1
Acc  88.160
rMSE 0.09834698520028765
channel_pruning 1460.5351564884186
Extracting X relu4_2 From Y conv4_3 stride 1
Acc  87.880
spatial_decomposition 13165.02792429924
run for 500 batches nFeatsPerBatch 100
Extracting conv4_3 (50000, 512)
Acc  87.760
Reconstruction Err 0.20429329902444401
channel_decomposition 4942.049018859863
Extracting X pool4 From Y conv5_1 stride 1
Acc  87.780
spatial_decomposition 1608.7209899425507
run for 500 batches nFeatsPerBatch 100
Extracting conv5_1 (50000, 512)
Acc  87.840
Reconstruction Err 0.19433694514953245
channel_decomposition 991.0077049732208
Extracting X relu5_1 From Y conv5_2 stride 1
Acc  87.900
spatial_decomposition 1551.2839875221252
run for 500 batches nFeatsPerBatch 100
Extracting conv5_2 (50000, 512)
Acc  87.440
Reconstruction Err 0.210634351133129
channel_decomposition 983.0862488746643
Extracting X relu5_2 From Y conv5_3 stride 1
Acc  87.460
spatial_decomposition 1521.1255655288696
run for 500 batches nFeatsPerBatch 100
Extracting conv5_3 (50000, 512)
Acc  87.400
Reconstruction Err 0.21809377028564134
channel_decomposition 974.4733574390411
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
stage2 saving
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[libprotobuf WARNING google/protobuf/io/coded_stream.cc:604] Reading dangerously large protocol message.  If the message turns out to be larger than 2147483647 bytes, parsing will be halted for security reasons.  To increase the limit (or to disable these warnings), see CodedInputStream::SetTotalBytesLimit() in google/protobuf/io/coded_stream.h.
[libprotobuf WARNING google/protobuf/io/coded_stream.cc:81] The total number of bytes read was 553432081
caffe test -model temp/3c_3C4x_mem_bn_vgg.prototxt -weights temp/3c_vgg.caffemodel
```

```
flops 15346630656
conv1_1 86704128 5
conv1_2 1849688064 120
conv2_1 924844032 60
conv2_2 1849688064 120
conv3_1 924844032 60
conv3_2 1849688064 120
conv3_3 1849688064 120
conv4_1 924844032 60
conv4_2 1849688064 120
conv4_3 1849688064 120
conv5_1 462422016 30
conv5_2 462422016 30
conv5_3 462422016 30
100.0


orig 15346630656
flops 4873358112
conv1_1 86704128 17
conv1_2_V 211943424 43
conv1_2_H 72855552 14
conv1_2_P 64024576 13
conv2_1_V 106950144 21
conv2_1_H 90354432 18
conv2_1_P 71914752 14
conv2_2_V 272982528 56
conv2_2_H 144657408 29
conv2_2_P 91771904 18
conv3_1_V 122115840 25
conv3_1_H 113836800 23
conv3_1_P 78305920 16
conv3_2_V 252002688 51
conv3_2_H 130996992 26
conv3_2_P 85851136 17
conv3_3_V 307754496 63
conv3_3_H 187040448 38
conv3_3_P 113197056 23
conv4_1_V 140292096 28
conv4_1_H 127687728 26
conv4_1_P 86403856 17
conv4_2_V 284798976 58
conv4_2_H 154140672 31
conv4_2_P 96137216 19
conv4_3_V 340235616 69
conv4_3_H 214511808 44
conv4_3_P 121225216 24
conv5_1_V 119820288 24
conv5_1_H 119820288 24
conv5_2_V 117411840 24
conv5_2_H 117411840 24
conv5_3_V 114100224 23
conv5_3_H 114100224 23
31.755231628609543
```

```
./calflop.sh -model temp/cb_3c_3C4x_mem_bn_vgg.prototxt -weights temp/cb_3c_vgg.caffemodel
```