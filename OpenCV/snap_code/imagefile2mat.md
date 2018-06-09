# Image file convert to cv::Mat

``` cpp
#include <iostream>

#include <opencv2/highgui/highgui.hpp>
#include <opencv2/imgproc/imgproc.hpp>

/**
  @brief: 图片文件 byte 数组转 cv::Mat 格式，支持 jpg、png、bmp 等格式图片
  @param:  data   (input)  图片文件 byte 数组
  @param:  length (input)  图片文件 byte 数组长度
  @param:  dst    (output) 返回 cv::Mat 对象
  @return: 返回 0 正确，其他失败
*/
int DecodeImageFileToMat(const unsigned char *data,
                         const int length,
                         cv::Mat &dst) {
  if (data == NULL || length < 1) {
    return -1;
  }
  cv::Mat image_buf(cv::Size(length, 1), CV_8UC3);
  memcpy(image_buf.data, data, length);
  dst = imdecode(image_buf, CV_LOAD_IMAGE_COLOR);
  return 0;
}

// TEST
int main(int argc, char** argv) {
  

  FILE *fp = fopen(argv[1], "r");
  if (NULL == fp) {
    std::cout << "error: read file failed" << std::endl;
    exit(-1);
  }
  // 获取文件字节大小
  fseek(fp, 0, SEEK_END);
  int size = ftell(fp);
  fseek(fp, 0, SEEK_SET);

  unsigned char *p = NULL;
  if(size > 0) {
    std::cout << "file size: " << size << std::endl;
    p = new unsigned char[size];
  } else {
    std::cout << "error: file size < 1" << std::endl;
    exit(-1);
  }
  // 读取并将文件字节流存入内存
  fread(p, size, 1, fp);

  // 测试 DecodeImageFileToMat 
  cv::Mat image;
  DecodeImageFileToMat(p, size, image);

  std::cout << "width:  " << image.cols << std::endl;
  std::cout << "height: " << image.rows << std::endl;

  if (image.data) {
    cv::imshow("mat show", image);
    cv::waitKey(0);
  } else {
    std::cout << "error: Mat.data is null" << std::endl;
    exit(-1);
  }

  fclose(fp);
  if (p != NULL) {
    delete p;
    p = NULL;
  }
  return 0;
}
```