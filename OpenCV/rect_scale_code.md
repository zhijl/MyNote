# 获取图像局部区域并向外扩展一定比例

## 代码

```
#include <opencv2/opencv.hpp>
#include <iostream>

void ScaleRect(const cv::Mat &image,
               cv::Mat *out,
               const cv::Rect &src,
               cv::Rect *dst,
               float scale = 0.1f) {
  int w_inc = static_cast<int>(src.width * scale);
  int h_inc = static_cast<int>(src.height * scale);

  dst->x = src.x - w_inc < 0 ? 0 : src.x - w_inc;
  dst->y = src.y - h_inc < 0 ? 0 : src.y - h_inc;
  dst->width = src.width + dst->x + w_inc  > image.cols
                  ? image.cols : src.width + w_inc - 1;
  dst->height = src.height + dst->y + h_inc > image.rows - 1
                  ? image.rows - 1 : src.height + h_inc - 1;
  std::cout << dst->width << std::endl;
  std::cout << dst->height << std::endl;
  image(*dst).copyTo(*out);
}

int main() {
  cv::Mat image = cv::imread("/home/zhi/321.jpg");

  std::cout << image.cols << "  " << image.rows << std::endl;

  cv::Rect rect(100, 100, 100, 100);
  cv::Rect rect_scale;
  cv::Mat image_scale;

  ScaleRect(image, &image_scale, rect, &rect_scale, 8);

//  cv::rectangle(image, rect, cv::Scalar_<double>(0, 255, 255), 5);

  cv::imshow("1", image_scale);
  cv::waitKey(0);

  return 0;
}
```

2018年01月11日11:24:10
