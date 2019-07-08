# CMake 在 Windows 平台使用时的问题记录

### 直接 cmake -G "NMake Makefiles" 报错

错误日志如下：

```
$ cmake -G "NMake Makefiles" ..
-- The C compiler identification is MSVC 19.0.24215.1
-- The CXX compiler identification is MSVC 19.0.24215.1
-- Check for working C compiler: D:/Program Files (x86)/Microsoft Visual Studio                                                    14.0/VC/bin/cl.exe
-- Check for working C compiler: D:/Program Files (x86)/Microsoft Visual Studio                                                    14.0/VC/bin/cl.exe -- broken
CMake Error at D:/Program Files/CMake/share/cmake-3.9/Modules/CMakeTestCCompiler                                                   .cmake:51 (message):
  The C compiler "D:/Program Files (x86)/Microsoft Visual Studio
  14.0/VC/bin/cl.exe" is not able to compile a simple test program.

  It fails with the following output:

   Change Dir: D:/wavelib-face-detection-07/build/CMakeFiles/CMakeTmp



  Run Build Command:"nmake" "/NOLOGO" "cmTC_88725\fast"

        "D:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin\nmake.exe" -                                                   f
  CMakeFiles\cmTC_88725.dir\build.make /nologo -L
  CMakeFiles\cmTC_88725.dir\build

  Building C object CMakeFiles/cmTC_88725.dir/testCCompiler.c.obj

        "D:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin\cl.exe"
  @C:\Users\ZhiJL\AppData\Local\Temp\nm12D7.tmp

  testCCompiler.c

  Linking C executable cmTC_88725.exe

        "D:\Program Files\CMake\bin\cmake.exe" -E vs_link_exe
  --intdir=CMakeFiles\cmTC_88725.dir --manifests -- "D:\Program Files
  (x86)\Microsoft Visual Studio 14.0\VC\bin\link.exe" /nologo
  @CMakeFiles\cmTC_88725.dir\objects1.rsp
  @C:\Users\ZhiJL\AppData\Local\Temp\nm1307.tmp

  RC Pass 1 failed to run.

  NMAKE : fatal error U1077: '"D:\Program Files\CMake\bin\cmake.exe"' :
  return code '0xffffffff'

  Stop.

  NMAKE : fatal error U1077: '"D:\Program Files (x86)\Microsoft Visual Studio
  14.0\VC\bin\nmake.exe"' : return code '0x2'

  Stop.





  CMake will not be able to correctly generate this project.
Call Stack (most recent call first):
  CMakeLists.txt:18 (project)


-- Configuring incomplete, errors occurred!
See also "D:/wavelib-face-detection-07/build/CMakeFiles/CMakeOutput.log".
See also "D:/wavelib-face-detection-07/build/CMakeFiles/CMakeError.log".
```

解决方法就是需要先配置下 VC 编译器的一些参数，这个直接执行 VC 下的 `vcvarsall.bat` 脚本即可，**注意这里需要在 cmd 中进行操作**，也即后面的 cmake 执行也只能在当前的 cmd 中操作

```
D:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\ vcvarsall.bat
```

然后再 `cmake -G "NMake Makefiles"` 就不会有那样的问题了

当然，也可以直接打开 `VS201x x86 Native Tools Command Prompt` ，在这个 cmd 中直接跑 `cmake -G "NMake Makefiles"` 是没问题的

参考：

- [CMake“NMake Makefile”生成器无法编译](https://cloud.tencent.com/developer/ask/198934/answer/309934)

