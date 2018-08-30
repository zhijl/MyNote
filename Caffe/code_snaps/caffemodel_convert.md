# Caffe prototxt 与 caffemodel 合并

``` cpp
//
// Created by zhi on 08/30/18.
// Describe: Convert caffe protofile and model to single binary model file
//

#include <caffe/net.hpp>
#include <caffe/util/io.hpp>

void CaffeProtoModel2Binary(const std::string &proto_txt,
                            const std::string &proto_model,
                            const std::string &bin_model) {
  boost::shared_ptr<caffe::Net<float> >
      net(new caffe::Net<float>(proto_txt, caffe::TEST));
  net->CopyTrainedLayersFromBinaryProto(proto_model);
  caffe::NetParameter param;
  net->ToProto(&param, false);
  caffe::WriteProtoToBinaryFile(param, bin_model);
}

int main (int argc, char** argv) {
  if (argc != 4) {
    std::cerr << "[Usage]: " << argv[0]
              << " prototxt_path caffemodel_path binary_name" << std::endl;
    std::cerr << "[Example]: " << argv[0]
              << " Alexnet_deploy.prototxt Alexnet_20000.caffemodel"
                 " Alexnet_v1.0.0.bin" << std::endl;
    std::cerr << "[Platform]:  Ubuntu 16.04" << std::endl;
    exit(-1);
  }

  CaffeProtoModel2Binary(argv[1], argv[2], argv[3]);

  return 0;
}
```