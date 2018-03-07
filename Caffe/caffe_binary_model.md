## Caffe 模型转二进制文件

``` cpp
  boost::shared_ptr<caffe::Net<float> >
      net(new caffe::Net<float>("../models/deploy.prototxt", caffe::TEST));
  net->blob_by_name("data")->Reshape(1, 1, 1000, 1000);
  net->CopyTrainedLayersFrom("../models/iter_1200000.caffemodel");
  caffe::NetParameter param;
  net->ToProto(&param, false);
  caffe::WriteProtoToBinaryFile(param, "model_linux64_cpu_v1.0.bat");
```