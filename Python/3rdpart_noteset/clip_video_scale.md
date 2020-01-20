# 裁剪视屏长宽尺度

需要预装 `python-opencv` 和 `ffmpeg`

``` sh
# split video and bgm
ffmpeg -i 7dec2015bf61937c2466935d52305720.mp4 -vn -y -acodec copy bgm.aac 
# merge video with bgm
ffmpeg -i test2.avi -i bgm.aac -vcodec copy -acodec copy output.avi    
```

``` python
#coding=utf-8  
import cv2  

# Read input video
videoCapture = cv2.VideoCapture("7dec2015bf61937c2466935d52305720.mp4")

# Get origin FPS parameter
src_fps = videoCapture.get(cv2.CAP_PROP_FPS)
src_size = (int(videoCapture.get(cv2.CAP_PROP_FRAME_WIDTH)),
  int(videoCapture.get(cv2.CAP_PROP_FRAME_HEIGHT)))

print('SRC FPS:  {}'.format(src_fps))
print('SRC size: {}'.format(src_size))

# Costum size
dst_fps = src_fps
dst_size = (720, int(1280 * 0.7) - 115)

# Write image to video by costum size
videoWriter = cv2.VideoWriter('output.avi', cv2.VideoWriter_fourcc('X', 'V', 'I', 'D'), dst_fps, dst_size)  
success, frame = videoCapture.read()#读取第一帧  

print('DST DPS:  {}'.format(dst_fps))
print('DST size: {}'.format(frame.size))

while success:  
  frame = frame[115 : int(1280 * 0.7), :, :]
  videoWriter.write(frame)
  success, frame = videoCapture.read()
videoCapture.release()
```
