# 记录一些使用 Intel 编译器和 MKL 库的问题

### 使用 icc 编译器链接库的问题

- `icc -help` 显示更多使用信息
- 可以直接通过 `-mkl` 选项链接已经安装的 MKL 库，一般优先链接动态库，如果想链接动态库，则在链接时需要全称 libmkl_xxx.a 表明调用静态库
- 可以通过这个页面生成想要的编译参数项 `https://software.intel.com/content/www/us/en/develop/articles/intel-mkl-link-line-advisor.html`
- 静态链接 `icc` 的标准库，链接时加入选项 `-static-intel`