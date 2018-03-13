# C++ 问题总结

## ARM 平台的交叉编译中出现的 xx.so VFP register arguments, output does not 问题

这句话提示的是在 xx.so 动态链接库中使用的时 hard floatting points 和 VFP unit，但是项目在编译时使用的是 soft floating points 。解决的方法就是在编译时，加上 -mfloat-abi=hard，-mfpu=vfp，或者重新以 soft floating points 的方式重新编译 xx.so 动态链接库，具体可参考 gcc 选项中的相关说明。

> 参考 https://stackoverflow.com/questions/22989766/libopencl-so-uses-vfp-register-arguments-output-does-not#