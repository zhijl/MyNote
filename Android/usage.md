# 一些 Android 开发小技巧

### LogCat

让 logcat 日志显示在终端上，主要是有时候出 bug 时，App 重启， Android Studio 中的 Logcat 被清空

``` shell
adb logcat
```

### Android.mk 日志输出

在 Android.mk 中输出日志

``` Makefile
$(warning string)
$(error string)
$(info string)
```

例如：

``` Makefile
LOCAL_PATH:= $(call my-dir)

include $(CLEAR_VARS)

LOCAL_MODULE    := libgl2jni
LOCAL_CFLAGS    := -Werror
LOCAL_SRC_FILES := gl_code.cpp
LOCAL_LDLIBS    := -llog -lGLESv2

$(warning 'hehe fuck')
$(warning $(LOCAL_SRC_FILES),$(LOCAL_PATH))
$(warning $(all-subdir-makefiles),$(this.makefile))
$(warning $(TARGET_ARCH))
$(info erroinfo,hehe)
$(message msg)
$(warning  " LOCAL_LDLIBS =  $(LOCAL_LDLIBS)")
$(info $(TARGET_PLATFORM))

include $(BUILD_SHARED_LIBRARY)
```
