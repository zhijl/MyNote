# 解读 Caffe 项目

写这个的目的主要是为了深入阅读 Caffe 的源代码，记录阅读过程中的笔记，一年前也尝试阅读 Caffe2 的代码，但后面因为其他项目的开展就停止了相关的阅读学习。而后在过去半年的时间里，多次用到 Caffe 进行网络的训练和网络的部署，其中还折腾各种 Caffe 的交叉编译，遇到了很多问题，一时感觉自己的知识真心不够用，而对于 Caffe 的使用也仅仅只是基于能用起来的程度，而对于其中的很多细节的实现却不是很清楚（比如按自己的想法修改和加入新的网络层或程序），所以很希望能够彻底地阅读这个项目的源代码，包括项目的组织结构、构建细节，以方便以后熟练使用 Caffe，修改 Caffe，或者进一步打造一个更佳的深度学习框架。有点扯远了，希望这次自己能坚持完成 Caffe 的源码阅读。整个过程会花很长时间，记录的过程一开始也会比较乱，但希望后面能逐步整理、修正和完善成一个比较好看的版本:)

> TODO： 探索一款满意的画图工具、如果能够用编程的方式画图并且画面自然风格（好看很重要）还能支持动画制作，那就太好了

---

开始时间： 2018年07月28日

Caffe 版本： commit 2a1c552b66f026c7508d390b526f2495ed3be594

GitHub 地址： https://github.com/BVLC/caffe/tree/2a1c552b66f026c7508d390b526f2495ed3be594

这个过程中使用的系统平台和工具有 Ubuntu 16.04、Sublime Text 3、VIM、grep、VS Code（编辑此文）等工具

### Caffe 目录结构及所有文件预览

首先查看一下整个项目的目录结构

``` text
caffe/
├── caffe.cloc
├── cmake
│   ├── ConfigGen.cmake
│   ├── Cuda.cmake
│   ├── Dependencies.cmake
│   ├── External
│   │   ├── gflags.cmake
│   │   └── glog.cmake
│   ├── lint.cmake
│   ├── Misc.cmake
│   ├── Modules
│   │   ├── FindAtlas.cmake
│   │   ├── FindGFlags.cmake
│   │   ├── FindGlog.cmake
│   │   ├── FindLAPACK.cmake
│   │   ├── FindLevelDB.cmake
│   │   ├── FindLMDB.cmake
│   │   ├── FindMatlabMex.cmake
│   │   ├── FindMKL.cmake
│   │   ├── FindNCCL.cmake
│   │   ├── FindNumPy.cmake
│   │   ├── FindOpenBLAS.cmake
│   │   ├── FindSnappy.cmake
│   │   └── FindvecLib.cmake
│   ├── ProtoBuf.cmake
│   ├── Summary.cmake
│   ├── Targets.cmake
│   ├── Templates
│   │   ├── CaffeConfig.cmake.in
│   │   ├── caffe_config.h.in
│   │   └── CaffeConfigVersion.cmake.in
│   ├── Uninstall.cmake.in
│   └── Utils.cmake
├── CMakeLists.txt
├── CONTRIBUTING.md
├── CONTRIBUTORS.md
├── data
│   ├── cifar10
│   │   └── get_cifar10.sh
│   ├── ilsvrc12
│   │   └── get_ilsvrc_aux.sh
│   └── mnist
│       └── get_mnist.sh
├── docker
│   ├── cpu
│   │   └── Dockerfile
│   ├── gpu
│   │   └── Dockerfile
│   └── README.md
├── docs
│   ├── CMakeLists.txt
│   ├── CNAME
│   ├── _config.yml
│   ├── development.md
│   ├── images
│   │   ├── caffeine-icon.png
│   │   └── GitHub-Mark-64px.png
│   ├── index.md
│   ├── install_apt_debian.md
│   ├── install_apt.md
│   ├── installation.md
│   ├── install_osx.md
│   ├── install_yum.md
│   ├── _layouts
│   │   └── default.html
│   ├── model_zoo.md
│   ├── multigpu.md
│   ├── README.md
│   ├── stylesheets
│   │   ├── pygment_trac.css
│   │   ├── reset.css
│   │   └── styles.css
│   └── tutorial
│       ├── convolution.md
│       ├── data.md
│       ├── fig
│       │   ├── backward.jpg
│       │   ├── forward_backward.png
│       │   ├── forward.jpg
│       │   ├── layer.jpg
│       │   └── logreg.jpg
│       ├── forward_backward.md
│       ├── index.md
│       ├── interfaces.md
│       ├── layers
│       │   ├── absval.md
│       │   ├── accuracy.md
│       │   ├── argmax.md
│       │   ├── batchnorm.md
│       │   ├── batchreindex.md
│       │   ├── bias.md
│       │   ├── bnll.md
│       │   ├── concat.md
│       │   ├── contrastiveloss.md
│       │   ├── convolution.md
│       │   ├── crop.md
│       │   ├── data.md
│       │   ├── deconvolution.md
│       │   ├── dropout.md
│       │   ├── dummydata.md
│       │   ├── eltwise.md
│       │   ├── elu.md
│       │   ├── embed.md
│       │   ├── euclideanloss.md
│       │   ├── exp.md
│       │   ├── filter.md
│       │   ├── flatten.md
│       │   ├── hdf5data.md
│       │   ├── hdf5output.md
│       │   ├── hingeloss.md
│       │   ├── im2col.md
│       │   ├── imagedata.md
│       │   ├── infogainloss.md
│       │   ├── innerproduct.md
│       │   ├── input.md
│       │   ├── log.md
│       │   ├── lrn.md
│       │   ├── lstm.md
│       │   ├── memorydata.md
│       │   ├── multinomiallogisticloss.md
│       │   ├── mvn.md
│       │   ├── parameter.md
│       │   ├── pooling.md
│       │   ├── power.md
│       │   ├── prelu.md
│       │   ├── python.md
│       │   ├── recurrent.md
│       │   ├── reduction.md
│       │   ├── relu.md
│       │   ├── reshape.md
│       │   ├── rnn.md
│       │   ├── scale.md
│       │   ├── sigmoidcrossentropyloss.md
│       │   ├── sigmoid.md
│       │   ├── silence.md
│       │   ├── slice.md
│       │   ├── softmax.md
│       │   ├── softmaxwithloss.md
│       │   ├── split.md
│       │   ├── spp.md
│       │   ├── tanh.md
│       │   ├── threshold.md
│       │   ├── tile.md
│       │   └── windowdata.md
│       ├── layers.md
│       ├── loss.md
│       ├── net_layer_blob.md
│       └── solver.md
├── examples
│   ├── 00-classification.ipynb
│   ├── 01-learning-lenet.ipynb
│   ├── 02-fine-tuning.ipynb
│   ├── brewing-logreg.ipynb
│   ├── cifar10
│   │   ├── cifar10_full.prototxt
│   │   ├── cifar10_full_sigmoid_solver_bn.prototxt
│   │   ├── cifar10_full_sigmoid_solver.prototxt
│   │   ├── cifar10_full_sigmoid_train_test_bn.prototxt
│   │   ├── cifar10_full_sigmoid_train_test.prototxt
│   │   ├── cifar10_full_solver_lr1.prototxt
│   │   ├── cifar10_full_solver_lr2.prototxt
│   │   ├── cifar10_full_solver.prototxt
│   │   ├── cifar10_full_train_test.prototxt
│   │   ├── cifar10_quick.prototxt
│   │   ├── cifar10_quick_solver_lr1.prototxt
│   │   ├── cifar10_quick_solver.prototxt
│   │   ├── cifar10_quick_train_test.prototxt
│   │   ├── convert_cifar_data.cpp
│   │   ├── create_cifar10.sh
│   │   ├── readme.md
│   │   ├── train_full.sh
│   │   ├── train_full_sigmoid_bn.sh
│   │   ├── train_full_sigmoid.sh
│   │   └── train_quick.sh
│   ├── CMakeLists.txt
│   ├── cpp_classification
│   │   ├── classification.cpp
│   │   └── readme.md
│   ├── detection.ipynb
│   ├── feature_extraction
│   │   ├── imagenet_val.prototxt
│   │   └── readme.md
│   ├── finetune_flickr_style
│   │   ├── assemble_data.py
│   │   ├── flickr_style.csv.gz
│   │   ├── readme.md
│   │   └── style_names.txt
│   ├── finetune_pascal_detection
│   │   ├── pascal_finetune_solver.prototxt
│   │   └── pascal_finetune_trainval_test.prototxt
│   ├── hdf5_classification
│   │   ├── nonlinear_auto_test.prototxt
│   │   ├── nonlinear_auto_train.prototxt
│   │   ├── nonlinear_train_val.prototxt
│   │   └── train_val.prototxt
│   ├── imagenet
│   │   ├── create_imagenet.sh
│   │   ├── make_imagenet_mean.sh
│   │   ├── readme.md
│   │   ├── resume_training.sh
│   │   └── train_caffenet.sh
│   ├── images
│   │   ├── cat gray.jpg
│   │   ├── cat_gray.jpg
│   │   ├── cat.jpg
│   │   └── fish-bike.jpg
│   ├── mnist
│   │   ├── convert_mnist_data.cpp
│   │   ├── create_mnist.sh
│   │   ├── lenet_adadelta_solver.prototxt
│   │   ├── lenet_auto_solver.prototxt
│   │   ├── lenet_consolidated_solver.prototxt
│   │   ├── lenet_multistep_solver.prototxt
│   │   ├── lenet.prototxt
│   │   ├── lenet_solver_adam.prototxt
│   │   ├── lenet_solver.prototxt
│   │   ├── lenet_solver_rmsprop.prototxt
│   │   ├── lenet_train_test.prototxt
│   │   ├── mnist_autoencoder.prototxt
│   │   ├── mnist_autoencoder_solver_adadelta.prototxt
│   │   ├── mnist_autoencoder_solver_adagrad.prototxt
│   │   ├── mnist_autoencoder_solver_nesterov.prototxt
│   │   ├── mnist_autoencoder_solver.prototxt
│   │   ├── readme.md
│   │   ├── train_lenet_adam.sh
│   │   ├── train_lenet_consolidated.sh
│   │   ├── train_lenet_docker.sh
│   │   ├── train_lenet_rmsprop.sh
│   │   ├── train_lenet.sh
│   │   ├── train_mnist_autoencoder_adadelta.sh
│   │   ├── train_mnist_autoencoder_adagrad.sh
│   │   ├── train_mnist_autoencoder_nesterov.sh
│   │   └── train_mnist_autoencoder.sh
│   ├── net_surgery
│   │   ├── bvlc_caffenet_full_conv.prototxt
│   │   └── conv.prototxt
│   ├── net_surgery.ipynb
│   ├── pascal-multilabel-with-datalayer.ipynb
│   ├── pycaffe
│   │   ├── caffenet.py
│   │   ├── layers
│   │   │   ├── pascal_multilabel_datalayers.py
│   │   │   └── pyloss.py
│   │   ├── linreg.prototxt
│   │   └── tools.py
│   ├── siamese
│   │   ├── convert_mnist_siamese_data.cpp
│   │   ├── create_mnist_siamese.sh
│   │   ├── mnist_siamese.ipynb
│   │   ├── mnist_siamese.prototxt
│   │   ├── mnist_siamese_solver.prototxt
│   │   ├── mnist_siamese_train_test.prototxt
│   │   ├── readme.md
│   │   └── train_mnist_siamese.sh
│   └── web_demo
│       ├── app.py
│       ├── exifutil.py
│       ├── readme.md
│       ├── requirements.txt
│       └── templates
│           └── index.html
├── include
│   └── caffe
│       ├── blob.hpp
│       ├── caffe.hpp
│       ├── common.hpp
│       ├── data_transformer.hpp
│       ├── filler.hpp
│       ├── internal_thread.hpp
│       ├── layer_factory.hpp
│       ├── layer.hpp
│       ├── layers
│       │   ├── absval_layer.hpp
│       │   ├── accuracy_layer.hpp
│       │   ├── argmax_layer.hpp
│       │   ├── base_conv_layer.hpp
│       │   ├── base_data_layer.hpp
│       │   ├── batch_norm_layer.hpp
│       │   ├── batch_reindex_layer.hpp
│       │   ├── bias_layer.hpp
│       │   ├── bnll_layer.hpp
│       │   ├── concat_layer.hpp
│       │   ├── contrastive_loss_layer.hpp
│       │   ├── conv_layer.hpp
│       │   ├── crop_layer.hpp
│       │   ├── cudnn_conv_layer.hpp
│       │   ├── cudnn_deconv_layer.hpp
│       │   ├── cudnn_lcn_layer.hpp
│       │   ├── cudnn_lrn_layer.hpp
│       │   ├── cudnn_pooling_layer.hpp
│       │   ├── cudnn_relu_layer.hpp
│       │   ├── cudnn_sigmoid_layer.hpp
│       │   ├── cudnn_softmax_layer.hpp
│       │   ├── cudnn_tanh_layer.hpp
│       │   ├── data_layer.hpp
│       │   ├── deconv_layer.hpp
│       │   ├── dropout_layer.hpp
│       │   ├── dummy_data_layer.hpp
│       │   ├── eltwise_layer.hpp
│       │   ├── elu_layer.hpp
│       │   ├── embed_layer.hpp
│       │   ├── euclidean_loss_layer.hpp
│       │   ├── exp_layer.hpp
│       │   ├── filter_layer.hpp
│       │   ├── flatten_layer.hpp
│       │   ├── hdf5_data_layer.hpp
│       │   ├── hdf5_output_layer.hpp
│       │   ├── hinge_loss_layer.hpp
│       │   ├── im2col_layer.hpp
│       │   ├── image_data_layer.hpp
│       │   ├── infogain_loss_layer.hpp
│       │   ├── inner_product_layer.hpp
│       │   ├── input_layer.hpp
│       │   ├── log_layer.hpp
│       │   ├── loss_layer.hpp
│       │   ├── lrn_layer.hpp
│       │   ├── lstm_layer.hpp
│       │   ├── memory_data_layer.hpp
│       │   ├── multinomial_logistic_loss_layer.hpp
│       │   ├── mvn_layer.hpp
│       │   ├── neuron_layer.hpp
│       │   ├── parameter_layer.hpp
│       │   ├── pooling_layer.hpp
│       │   ├── power_layer.hpp
│       │   ├── prelu_layer.hpp
│       │   ├── python_layer.hpp
│       │   ├── recurrent_layer.hpp
│       │   ├── reduction_layer.hpp
│       │   ├── relu_layer.hpp
│       │   ├── reshape_layer.hpp
│       │   ├── rnn_layer.hpp
│       │   ├── scale_layer.hpp
│       │   ├── sigmoid_cross_entropy_loss_layer.hpp
│       │   ├── sigmoid_layer.hpp
│       │   ├── silence_layer.hpp
│       │   ├── slice_layer.hpp
│       │   ├── softmax_layer.hpp
│       │   ├── softmax_loss_layer.hpp
│       │   ├── split_layer.hpp
│       │   ├── spp_layer.hpp
│       │   ├── swish_layer.hpp
│       │   ├── tanh_layer.hpp
│       │   ├── threshold_layer.hpp
│       │   ├── tile_layer.hpp
│       │   └── window_data_layer.hpp
│       ├── net.hpp
│       ├── parallel.hpp
│       ├── sgd_solvers.hpp
│       ├── solver_factory.hpp
│       ├── solver.hpp
│       ├── syncedmem.hpp
│       ├── test
│       │   ├── test_caffe_main.hpp
│       │   └── test_gradient_check_util.hpp
│       └── util
│           ├── benchmark.hpp
│           ├── blocking_queue.hpp
│           ├── cudnn.hpp
│           ├── db.hpp
│           ├── db_leveldb.hpp
│           ├── db_lmdb.hpp
│           ├── device_alternate.hpp
│           ├── format.hpp
│           ├── gpu_util.cuh
│           ├── hdf5.hpp
│           ├── im2col.hpp
│           ├── insert_splits.hpp
│           ├── io.hpp
│           ├── math_functions.hpp
│           ├── mkl_alternate.hpp
│           ├── nccl.hpp
│           ├── rng.hpp
│           ├── signal_handler.h
│           └── upgrade_proto.hpp
├── INSTALL.md
├── LICENSE
├── Makefile
├── Makefile.config.example
├── matlab
│   ├── +caffe
│   │   ├── Blob.m
│   │   ├── get_net.m
│   │   ├── get_solver.m
│   │   ├── imagenet
│   │   │   └── ilsvrc_2012_mean.mat
│   │   ├── io.m
│   │   ├── Layer.m
│   │   ├── Net.m
│   │   ├── private
│   │   │   ├── caffe_.cpp
│   │   │   ├── CHECK_FILE_EXIST.m
│   │   │   ├── CHECK.m
│   │   │   └── is_valid_handle.m
│   │   ├── reset_all.m
│   │   ├── run_tests.m
│   │   ├── set_device.m
│   │   ├── set_mode_cpu.m
│   │   ├── set_mode_gpu.m
│   │   ├── Solver.m
│   │   ├── +test
│   │   │   ├── test_io.m
│   │   │   ├── test_net.m
│   │   │   └── test_solver.m
│   │   └── version.m
│   ├── CMakeLists.txt
│   ├── demo
│   │   └── classification_demo.m
│   └── hdf5creation
│       ├── demo.m
│       └── store2hdf5.m
├── models
│   ├── bvlc_alexnet
│   │   ├── deploy.prototxt
│   │   ├── readme.md
│   │   ├── solver.prototxt
│   │   └── train_val.prototxt
│   ├── bvlc_googlenet
│   │   ├── deploy.prototxt
│   │   ├── quick_solver.prototxt
│   │   ├── readme.md
│   │   ├── solver.prototxt
│   │   └── train_val.prototxt
│   ├── bvlc_reference_caffenet
│   │   ├── deploy.prototxt
│   │   ├── readme.md
│   │   ├── solver.prototxt
│   │   └── train_val.prototxt
│   ├── bvlc_reference_rcnn_ilsvrc13
│   │   ├── deploy.prototxt
│   │   └── readme.md
│   └── finetune_flickr_style
│       ├── deploy.prototxt
│       ├── readme.md
│       ├── solver.prototxt
│       └── train_val.prototxt
├── python
│   ├── caffe
│   │   ├── _caffe.cpp
│   │   ├── classifier.py
│   │   ├── coord_map.py
│   │   ├── detector.py
│   │   ├── draw.py
│   │   ├── imagenet
│   │   │   └── ilsvrc_2012_mean.npy
│   │   ├── __init__.py
│   │   ├── io.py
│   │   ├── net_spec.py
│   │   ├── pycaffe.py
│   │   └── test
│   │       ├── test_coord_map.py
│   │       ├── test_draw.py
│   │       ├── test_io.py
│   │       ├── test_layer_type_list.py
│   │       ├── test_nccl.py
│   │       ├── test_net.py
│   │       ├── test_net_spec.py
│   │       ├── test_python_layer.py
│   │       ├── test_python_layer_with_param_str.py
│   │       └── test_solver.py
│   ├── classify.py
│   ├── CMakeLists.txt
│   ├── detect.py
│   ├── draw_net.py
│   ├── requirements.txt
│   └── train.py
├── README.md
├── scripts
│   ├── build_docs.sh
│   ├── caffe
│   ├── copy_notebook.py
│   ├── cpp_lint.py
│   ├── deploy_docs.sh
│   ├── download_model_binary.py
│   ├── download_model_from_gist.sh
│   ├── gather_examples.sh
│   ├── split_caffe_proto.py
│   ├── travis
│   │   ├── build.sh
│   │   ├── configure-cmake.sh
│   │   ├── configure-make.sh
│   │   ├── configure.sh
│   │   ├── defaults.sh
│   │   ├── install-deps.sh
│   │   ├── install-python-deps.sh
│   │   ├── setup-venv.sh
│   │   └── test.sh
│   └── upload_model_to_gist.sh
├── src
│   ├── caffe
│   │   ├── blob.cpp
│   │   ├── CMakeLists.txt
│   │   ├── common.cpp
│   │   ├── data_transformer.cpp
│   │   ├── internal_thread.cpp
│   │   ├── layer.cpp
│   │   ├── layer_factory.cpp
│   │   ├── layers
│   │   │   ├── absval_layer.cpp
│   │   │   ├── absval_layer.cu
│   │   │   ├── accuracy_layer.cpp
│   │   │   ├── accuracy_layer.cu
│   │   │   ├── argmax_layer.cpp
│   │   │   ├── base_conv_layer.cpp
│   │   │   ├── base_data_layer.cpp
│   │   │   ├── base_data_layer.cu
│   │   │   ├── batch_norm_layer.cpp
│   │   │   ├── batch_norm_layer.cu
│   │   │   ├── batch_reindex_layer.cpp
│   │   │   ├── batch_reindex_layer.cu
│   │   │   ├── bias_layer.cpp
│   │   │   ├── bias_layer.cu
│   │   │   ├── bnll_layer.cpp
│   │   │   ├── bnll_layer.cu
│   │   │   ├── concat_layer.cpp
│   │   │   ├── concat_layer.cu
│   │   │   ├── contrastive_loss_layer.cpp
│   │   │   ├── contrastive_loss_layer.cu
│   │   │   ├── conv_layer.cpp
│   │   │   ├── conv_layer.cu
│   │   │   ├── crop_layer.cpp
│   │   │   ├── crop_layer.cu
│   │   │   ├── cudnn_conv_layer.cpp
│   │   │   ├── cudnn_conv_layer.cu
│   │   │   ├── cudnn_deconv_layer.cpp
│   │   │   ├── cudnn_deconv_layer.cu
│   │   │   ├── cudnn_lcn_layer.cpp
│   │   │   ├── cudnn_lcn_layer.cu
│   │   │   ├── cudnn_lrn_layer.cpp
│   │   │   ├── cudnn_lrn_layer.cu
│   │   │   ├── cudnn_pooling_layer.cpp
│   │   │   ├── cudnn_pooling_layer.cu
│   │   │   ├── cudnn_relu_layer.cpp
│   │   │   ├── cudnn_relu_layer.cu
│   │   │   ├── cudnn_sigmoid_layer.cpp
│   │   │   ├── cudnn_sigmoid_layer.cu
│   │   │   ├── cudnn_softmax_layer.cpp
│   │   │   ├── cudnn_softmax_layer.cu
│   │   │   ├── cudnn_tanh_layer.cpp
│   │   │   ├── cudnn_tanh_layer.cu
│   │   │   ├── data_layer.cpp
│   │   │   ├── deconv_layer.cpp
│   │   │   ├── deconv_layer.cu
│   │   │   ├── dropout_layer.cpp
│   │   │   ├── dropout_layer.cu
│   │   │   ├── dummy_data_layer.cpp
│   │   │   ├── eltwise_layer.cpp
│   │   │   ├── eltwise_layer.cu
│   │   │   ├── elu_layer.cpp
│   │   │   ├── elu_layer.cu
│   │   │   ├── embed_layer.cpp
│   │   │   ├── embed_layer.cu
│   │   │   ├── euclidean_loss_layer.cpp
│   │   │   ├── euclidean_loss_layer.cu
│   │   │   ├── exp_layer.cpp
│   │   │   ├── exp_layer.cu
│   │   │   ├── filter_layer.cpp
│   │   │   ├── filter_layer.cu
│   │   │   ├── flatten_layer.cpp
│   │   │   ├── hdf5_data_layer.cpp
│   │   │   ├── hdf5_data_layer.cu
│   │   │   ├── hdf5_output_layer.cpp
│   │   │   ├── hdf5_output_layer.cu
│   │   │   ├── hinge_loss_layer.cpp
│   │   │   ├── im2col_layer.cpp
│   │   │   ├── im2col_layer.cu
│   │   │   ├── image_data_layer.cpp
│   │   │   ├── infogain_loss_layer.cpp
│   │   │   ├── inner_product_layer.cpp
│   │   │   ├── inner_product_layer.cu
│   │   │   ├── input_layer.cpp
│   │   │   ├── log_layer.cpp
│   │   │   ├── log_layer.cu
│   │   │   ├── loss_layer.cpp
│   │   │   ├── lrn_layer.cpp
│   │   │   ├── lrn_layer.cu
│   │   │   ├── lstm_layer.cpp
│   │   │   ├── lstm_unit_layer.cpp
│   │   │   ├── lstm_unit_layer.cu
│   │   │   ├── memory_data_layer.cpp
│   │   │   ├── multinomial_logistic_loss_layer.cpp
│   │   │   ├── mvn_layer.cpp
│   │   │   ├── mvn_layer.cu
│   │   │   ├── neuron_layer.cpp
│   │   │   ├── parameter_layer.cpp
│   │   │   ├── pooling_layer.cpp
│   │   │   ├── pooling_layer.cu
│   │   │   ├── power_layer.cpp
│   │   │   ├── power_layer.cu
│   │   │   ├── prelu_layer.cpp
│   │   │   ├── prelu_layer.cu
│   │   │   ├── recurrent_layer.cpp
│   │   │   ├── recurrent_layer.cu
│   │   │   ├── reduction_layer.cpp
│   │   │   ├── reduction_layer.cu
│   │   │   ├── relu_layer.cpp
│   │   │   ├── relu_layer.cu
│   │   │   ├── reshape_layer.cpp
│   │   │   ├── rnn_layer.cpp
│   │   │   ├── scale_layer.cpp
│   │   │   ├── scale_layer.cu
│   │   │   ├── sigmoid_cross_entropy_loss_layer.cpp
│   │   │   ├── sigmoid_cross_entropy_loss_layer.cu
│   │   │   ├── sigmoid_layer.cpp
│   │   │   ├── sigmoid_layer.cu
│   │   │   ├── silence_layer.cpp
│   │   │   ├── silence_layer.cu
│   │   │   ├── slice_layer.cpp
│   │   │   ├── slice_layer.cu
│   │   │   ├── softmax_layer.cpp
│   │   │   ├── softmax_layer.cu
│   │   │   ├── softmax_loss_layer.cpp
│   │   │   ├── softmax_loss_layer.cu
│   │   │   ├── split_layer.cpp
│   │   │   ├── split_layer.cu
│   │   │   ├── spp_layer.cpp
│   │   │   ├── swish_layer.cpp
│   │   │   ├── swish_layer.cu
│   │   │   ├── tanh_layer.cpp
│   │   │   ├── tanh_layer.cu
│   │   │   ├── threshold_layer.cpp
│   │   │   ├── threshold_layer.cu
│   │   │   ├── tile_layer.cpp
│   │   │   ├── tile_layer.cu
│   │   │   └── window_data_layer.cpp
│   │   ├── net.cpp
│   │   ├── parallel.cpp
│   │   ├── proto
│   │   │   └── caffe.proto
│   │   ├── solver.cpp
│   │   ├── solvers
│   │   │   ├── adadelta_solver.cpp
│   │   │   ├── adadelta_solver.cu
│   │   │   ├── adagrad_solver.cpp
│   │   │   ├── adagrad_solver.cu
│   │   │   ├── adam_solver.cpp
│   │   │   ├── adam_solver.cu
│   │   │   ├── nesterov_solver.cpp
│   │   │   ├── nesterov_solver.cu
│   │   │   ├── rmsprop_solver.cpp
│   │   │   ├── rmsprop_solver.cu
│   │   │   ├── sgd_solver.cpp
│   │   │   └── sgd_solver.cu
│   │   ├── syncedmem.cpp
│   │   ├── test
│   │   │   ├── CMakeLists.txt
│   │   │   ├── test_accuracy_layer.cpp
│   │   │   ├── test_argmax_layer.cpp
│   │   │   ├── test_batch_norm_layer.cpp
│   │   │   ├── test_batch_reindex_layer.cpp
│   │   │   ├── test_benchmark.cpp
│   │   │   ├── test_bias_layer.cpp
│   │   │   ├── test_blob.cpp
│   │   │   ├── test_caffe_main.cpp
│   │   │   ├── test_common.cpp
│   │   │   ├── test_concat_layer.cpp
│   │   │   ├── test_contrastive_loss_layer.cpp
│   │   │   ├── test_convolution_layer.cpp
│   │   │   ├── test_crop_layer.cpp
│   │   │   ├── test_data
│   │   │   │   ├── generate_sample_data.py
│   │   │   │   ├── sample_data_2_gzip.h5
│   │   │   │   ├── sample_data.h5
│   │   │   │   ├── sample_data_list.txt
│   │   │   │   ├── solver_data.h5
│   │   │   │   └── solver_data_list.txt
│   │   │   ├── test_data_layer.cpp
│   │   │   ├── test_data_transformer.cpp
│   │   │   ├── test_db.cpp
│   │   │   ├── test_deconvolution_layer.cpp
│   │   │   ├── test_dummy_data_layer.cpp
│   │   │   ├── test_eltwise_layer.cpp
│   │   │   ├── test_embed_layer.cpp
│   │   │   ├── test_euclidean_loss_layer.cpp
│   │   │   ├── test_filler.cpp
│   │   │   ├── test_filter_layer.cpp
│   │   │   ├── test_flatten_layer.cpp
│   │   │   ├── test_gradient_based_solver.cpp
│   │   │   ├── test_hdf5data_layer.cpp
│   │   │   ├── test_hdf5_output_layer.cpp
│   │   │   ├── test_hinge_loss_layer.cpp
│   │   │   ├── test_im2col_kernel.cu
│   │   │   ├── test_im2col_layer.cpp
│   │   │   ├── test_image_data_layer.cpp
│   │   │   ├── test_infogain_loss_layer.cpp
│   │   │   ├── test_inner_product_layer.cpp
│   │   │   ├── test_internal_thread.cpp
│   │   │   ├── test_io.cpp
│   │   │   ├── test_layer_factory.cpp
│   │   │   ├── test_lrn_layer.cpp
│   │   │   ├── test_lstm_layer.cpp
│   │   │   ├── test_math_functions.cpp
│   │   │   ├── test_maxpool_dropout_layers.cpp
│   │   │   ├── test_memory_data_layer.cpp
│   │   │   ├── test_multinomial_logistic_loss_layer.cpp
│   │   │   ├── test_mvn_layer.cpp
│   │   │   ├── test_net.cpp
│   │   │   ├── test_neuron_layer.cpp
│   │   │   ├── test_platform.cpp
│   │   │   ├── test_pooling_layer.cpp
│   │   │   ├── test_power_layer.cpp
│   │   │   ├── test_protobuf.cpp
│   │   │   ├── test_random_number_generator.cpp
│   │   │   ├── test_reduction_layer.cpp
│   │   │   ├── test_reshape_layer.cpp
│   │   │   ├── test_rnn_layer.cpp
│   │   │   ├── test_scale_layer.cpp
│   │   │   ├── test_sigmoid_cross_entropy_loss_layer.cpp
│   │   │   ├── test_slice_layer.cpp
│   │   │   ├── test_softmax_layer.cpp
│   │   │   ├── test_softmax_with_loss_layer.cpp
│   │   │   ├── test_solver.cpp
│   │   │   ├── test_solver_factory.cpp
│   │   │   ├── test_split_layer.cpp
│   │   │   ├── test_spp_layer.cpp
│   │   │   ├── test_stochastic_pooling.cpp
│   │   │   ├── test_syncedmem.cpp
│   │   │   ├── test_tanh_layer.cpp
│   │   │   ├── test_threshold_layer.cpp
│   │   │   ├── test_tile_layer.cpp
│   │   │   ├── test_upgrade_proto.cpp
│   │   │   └── test_util_blas.cpp
│   │   └── util
│   │       ├── benchmark.cpp
│   │       ├── blocking_queue.cpp
│   │       ├── cudnn.cpp
│   │       ├── db.cpp
│   │       ├── db_leveldb.cpp
│   │       ├── db_lmdb.cpp
│   │       ├── hdf5.cpp
│   │       ├── im2col.cpp
│   │       ├── im2col.cu
│   │       ├── insert_splits.cpp
│   │       ├── io.cpp
│   │       ├── math_functions.cpp
│   │       ├── math_functions.cu
│   │       ├── signal_handler.cpp
│   │       └── upgrade_proto.cpp
│   └── gtest
│       ├── CMakeLists.txt
│       ├── gtest-all.cpp
│       ├── gtest.h
│       └── gtest_main.cc
└── tools
    ├── caffe.cpp
    ├── CMakeLists.txt
    ├── compute_image_mean.cpp
    ├── convert_imageset.cpp
    ├── extra
    │   ├── extract_seconds.py
    │   ├── launch_resize_and_crop_images.sh
    │   ├── parse_log.py
    │   ├── parse_log.sh
    │   ├── plot_log.gnuplot.example
    │   ├── plot_training_log.py.example
    │   ├── resize_and_crop_images.py
    │   └── summarize.py
    ├── extract_features.cpp
    ├── upgrade_net_proto_binary.cpp
    ├── upgrade_net_proto_text.cpp
    └── upgrade_solver_proto_text.cpp

69 directories, 691 files
```

> TODO：在上面个别条目后加点内容

上面通过 tree 工具列出了整个项目目录下的所有子目录及文件，一共有691个文件（除隐藏文件），其中的隐藏文件主要为 `.gitignore`  `.git` `.travis.yml`。可见数量并没有多到惊人，所以要有信心:)

一级目录的描述如下：

``` text
caffe/
├── cmake           # CMake 模块文件，对于较大的工程，适合将不同部分的 CMakeLists 内容独立出来，方便维护
│   ├── External
│   ├── Modules
│   └── Templates
├── data            # 存放 example 样例程序中要用到的三个经典数据集，每个数据集通过脚本进行下载。后面会具体对这三个数据集进行介绍
│   ├── cifar10
│   ├── ilsvrc12
│   └── mnist
├── docker          # Docker 镜像构建文件，分为 cpu 和 gpu 两个版本
│   ├── cpu
│   └── gpu
├── docs            # 使用说明文档及教程文档
│   ├── images
│   ├── _layouts
│   ├── stylesheets
│   └── tutorial
│       ├── fig
│       └── layers
├── examples        # Caffe 样例
│   ├── cifar10
│   ├── cpp_classification
│   ├── feature_extraction
│   ├── finetune_flickr_style
│   ├── finetune_pascal_detection
│   ├── hdf5_classification
│   ├── imagenet
│   ├── images
│   ├── mnist
│   ├── net_surgery
│   ├── pycaffe
│   │   └── layers
│   ├── siamese
│   └── web_demo
│       └── templates
├── include        # Caffe 框架 C++ 头文件，这一部分很重要，是 Caffe 框架的外形
│   └── caffe
│       ├── layers
│       ├── test
│       └── util
├── matlab         # Caffe 框架 MATLBA 接口
│   ├── +caffe
│   │   ├── imagenet
│   │   ├── private
│   │   └── +test
│   ├── demo
│   └── hdf5creation
├── models         # 存放 example 中使用到的经典网络的结构文件
│   ├── bvlc_alexnet
│   ├── bvlc_googlenet
│   ├── bvlc_reference_caffenet
│   ├── bvlc_reference_rcnn_ilsvrc13
│   └── finetune_flickr_style
├── python         # Caffe 框架的 Python 接口
│   └── caffe
│       ├── imagenet
│       └── test
├── scripts        # Travis CI 脚本，后面会扩展 Travis 的相关介绍
│   └── travis
├── src            # Caffe 框架 C++ 源代码，这一部分也很重要，是 Caffe 整个框架内部的具体实现
│   ├── caffe
│   │   ├── layers
│   │   ├── proto
│   │   ├── solvers
│   │   ├── test
│   │   │   └── test_data
│   │   └── util
│   └── gtest
└── tools          # Caffe 的附属工具，使用这些工具可以快速轻便的实现一个网络的训练
    └── extra

69 directories
```

后面将重点分析两个目录，`include` 和`src`，不过准确的说第一步应该关注和分析的还有根目录下的其他文件，这些文件关于项目的各种配置和构建，所以这才是进入整个项目的导火线

根目录下的文件说明

``` text
.Doxyfile         # Doxyfile 文档生成工具的配置文件，关于 Doxyfile 适合最后再介绍
.gitignore        # 定义 git 中要忽略的一些文件
.travis.yml       # Travis 构建文件，同样会放在后面介绍，这个工具很有意思，以后自己的开源项目也要用起这个O(∩_∩)O~~
CMakeLists.txt    # 提供给 cmake 工具的构建文件，用于 CMake 构建整个项目，这个文件很重要，马上会细说
CONTRIBUTING.md   # 关于为项目做贡献的一些说明
CONTRIBUTORS.md   # 为项目做出贡献的人的清单（介绍列表获取方法）
INSTALL.md        # Caffe 的安装说明
Makefile          # 提供给 make 工具的构建文件，发挥的作用和 CMakeLists.txt 一样，用于组织整个项目的构建
Makefile.config.example  # Makefile 附属文件，Makefile 一开始会导入这个文件中定义的内容，主要用于配置 Makefile 中的某些参数项
README.md         # 项目的介绍说明
caffe.cloc        # cloc 也是一个工具，用于统计文件中的代码行数、注释行数等信息，所以这个文件就是给 cloc 用的
```

可见，Caffe 项目 支持使用 make 和 cmake 这两种工具构建，而 Caffe2 里面也有 Makefile 和 CMakeLists.txt，但 Caffe2 的 Makefile 中没有什么内容，而是直接地调用 cmake 使用项目底下的 CMakeLists.txt 构建项目。

> TODO：说明下 make 和 cmake 各自不同

make 的特点：

- make 使用的 Makefile 相当于一个较好的批处理工具，内容基本语法为目标+依赖+命令，只有在目标文件不存在，或目标比依赖的文件更旧，命令才会被执行。也就是说 make 可以不限于构建和编译一个项目，基础但是灵活

cmake 的优点：

- 用于编写 CMakeLists.txt 的规则和代码简单、容易理解
- 支持多种开发环境，Xcode/eclipse/Visual Studio
- 自动发现跨平台系统库
- 自动发现和管理工具集
- 共享库的生成更加方便

简单的说，如果仅仅只是为了在一个平台上构建一个很小的工程，那么 make 会更适合；而面对大工程、跨平台需求，则使用 cmake

1. [CMake和Make之间的区别](https://blog.csdn.net/android_ruben/article/details/51698498)
2. [make makefile cmake qmake都是什么，有什么区别？](https://www.zhihu.com/question/27455963)

### 解读 Caffe Makefile



### 解读 Caffe CMakeLists.txt


### 解读 Caffe 头文件


### 解读 Caffe 内部实现


