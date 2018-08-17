# PLog 日志库

PLog 是一个开源的相当强大的 C++ 日志库

GitHub 地址：[https://github.com/SergiusTheBest/plog](https://github.com/SergiusTheBest/plog)

## 特性

- 非常简洁。整个库只有 1000 多行代码
- 非常容易使用。只要引入头文件即可
- 线程安全和类型安全
- 无第三方依赖库。也无 windows.h 依赖
- 跨平台。支持 Windows、Linux、FreeBSD、macOS、Android、RTEMS（gcc，clang，msvc，mingw，mingw-w64，icc，c++build）
- 多种输出方式。不仅支持 shell 终端输出、滚动文件形式，还支持 Android 平台日志的终端输出
- 多种日志等级
- 日志文件格式，支持 TXT，CSV，FuncMessage
- 不需要 C++11
- 可扩展性强

## 使用方法

### 步骤一

在 C++ 项目中包含 PLog 头文件

``` cpp
#include <plog/Log.h>
```

### 步骤二

初始化日志系统

``` cpp
Logger& init(Severity max_severtiy,
             const char/wchar_t* file_name,
             size_t max_file_size = 0,
             int max_files = 0);
```

- `max_severtiy` 表示严重性的最大等级，如果某些日志的等级高于这个最高限制，这些日志将被忽略和移除，以避免输出
- `file_name` 日志文件名，日志的存储格式将取决于文件名的后缀，比如 `hello.txt` 或者 `hello.csv`
- `max_file_size` 表示单个日志文件最多存放的字节数，如果当个文件超过存储的日志超过该数目，依据日志滚动策略将后面的日志存放在另一个新的日志文件中
- `max_files` 表示最大的日志文件个数，如果 `max_file_size` 或者 `max_files` 任意一个为 0，那将不采用日志回滚策略，所有日志只存放在一个文件中

可选的日志严重性等级有：

``` cpp
enum Severity
{
    none = 0,
    fatal = 1,
    error = 2,
    warning = 3,
    info = 4,
    debug = 5,
    verbose = 6
};
```

`none` 等级的日志不受约束，总会输出

### 步骤三

添加日志，Plog 使用不同风格的宏作为接口帮助添加日志

长风格：

``` cpp
LOG_VERBOSE << "verbose";
LOG_DEBUG << "debug";
LOG_INFO << "info";
LOG_WARNING << "warning";
LOG_ERROR << "error";
LOG_FATAL << "fatal";
LOG_NONE << "none";
```

短风格：

``` cpp
LOGV << "verbose";
LOGD << "debug";
LOGI << "info";
LOGW << "warning";
LOGE << "error";
LOGF << "fatal";
LOGN << "none";
```

函数风格：

``` cpp
LOG(severity) << "msg";
```

长风格（条件日志）：

``` cpp
LOG_VERBOSE_IF(cond) << "verbose";
LOG_DEBUG_IF(cond) << "debug";
LOG_INFO_IF(cond) << "info";
LOG_WARNING_IF(cond) << "warning";
LOG_ERROR_IF(cond) << "error";
LOG_FATAL_IF(cond) << "fatal";
LOG_NONE_IF(cond) << "none";
```

短风格（条件日志）：

``` cpp
LOGV_IF(cond) << "verbose";
LOGD_IF(cond) << "debug";
LOGI_IF(cond) << "info";
LOGW_IF(cond) << "warning";
LOGE_IF(cond) << "error";
LOGF_IF(cond) << "fatal";
LOGN_IF(cond) << "none";
```

函数风格（条件日志）：

``` cpp
LOG_IF(severity, cond) << "msg";
```

### 日志等级检查

``` cpp
IF_LOG(severity)
```

例如：

``` cpp
IF_LOG(plog::debug) // we want to execute the following statements only at debug severity (and higher)
{
    for (int i = 0; i < vec.size(); ++i)
    {
        LOGD << "vec[" << i << "]: " << vec[i];
    }
}
```

### 一个简单的例子

``` cpp
#include <plog/Log.h> // Step1: include the header.

int main()
{
    plog::init(plog::debug, "Hello.txt"); // Step2: initialize the logger.

    // Step3: write log messages using a special macro. 
    // There are several log macros, use the macro you liked the most.

    LOGD << "Hello log!"; // short macro
    LOG_DEBUG << "Hello log!"; // long macro
    LOG(plog::debug) << "Hello log!"; // function-style macro

    return 0;
}
```

## 高级用法

### 运行时改变严重性等级

PLog 不是只有初始化时可以设置日志严重性级别，在运行时也能

接口如下：

``` cpp
# 获取当前等级
Severity Logger::getMaxSeverity() const;
# 设置日志等级
Logger::setMaxSeverity(Severity severity);
```

更多高级用法可参考 PLog 项目主页
