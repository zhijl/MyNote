# 解读 Caffe 项目

写这个的目的主要是为了深入阅读 Caffe 的源代码，记录阅读过程中的笔记，方便以后再次拾起 Caffe 的方方面面。一年前也尝试阅读 Caffe2 的代码，但后面因为其他项目的开展就停止了相关的阅读学习。而后在过去半年的时间里，多次用到 Caffe 进行网络的训练和网络的部署，其中还折腾各种 Caffe 的交叉编译，遇到了很多问题，一时感觉自己的知识真心不够用，而对于 Caffe 的使用也仅仅只是基于能用起来的程度，而对于其中的很多细节的实现却不是很清楚（比如按自己的想法修改和加入新的网络层或程序），所以很希望能够彻底地阅读这个项目的源代码，包括项目的组织结构、构建细节，以方便以后熟练使用 Caffe，修改 Caffe，或者进一步打造一个更佳的深度学习框架。有点扯远了，希望这次自己能坚持完成 Caffe 的源码阅读。整个过程会花很长时间，记录的过程一开始也会比较乱，但希望后面能逐步整理、修正和完善成一个比较好看的版本:)

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

**Reference**

1. [CMake和Make之间的区别](https://blog.csdn.net/android_ruben/article/details/51698498)
2. [make makefile cmake qmake都是什么，有什么区别？](https://www.zhihu.com/question/27455963)

### 解读 Caffe Makefile

前面有提到 Makefile 是打开 Caffe 项目的一个导火线，并且在 Caffe 中只有一个 Makefile，说明整个项目的所有程序的构建都由这个 Makefile 组织

##### [PART 01]

``` Makefile
PROJECT := caffe  # 定义项目名

CONFIG_FILE := Makefile.config  # Makefile 配置文件路径（Makefile.config在当前目录下）
# 检查是否存在 Makefile.config 这个文件，否则使用 make -k 命令终止 make 的执行
ifeq ($(wildcard $(CONFIG_FILE)),)  # 目的是判断是否存在 Makefile.config 这个文件
$(error $(CONFIG_FILE) not found. See $(CONFIG_FILE).example.) # error 会 stop make 的执行
endif
include $(CONFIG_FILE) # 将 Makefile.config 中的内容导入到此处
```

[NOTE]: 在 Makefile 规则中，通配符会被自动展开。但在变量的定义和函数引用时，通配符将失效。这种情况下如果需要通配符有效，就需要使用函数 “wildcard”，它的用法是：$(wildcard PATTERN...) 。在 Makefile 中，它被展开为已经存在的、使用空格分开的、匹配此模式的所有文件列表。如果不存在任何符合此模式的文件，函数会忽略模式字符并返回空
其他：
1、wildcard : 扩展通配符
2、notdir   : 去除路径     例如: dir=$(notdir $(src))
3、patsubst : 替换通配符，需要三个参数   例如: obj=$(patsubst %.c,%.o,$(dir)) # %.c 表示要替换的.c后缀，%.o 表示要替换的.o后缀，后面一项为要操作的列表
[NOTE]: ifeq 包含两个参数，用逗号隔开

> 下面从 Makefile.config 中的内容分析，因为里面有后面要用到的变量的定义

[Makefile.config.example]

``` Makefile
# 这个文件是为了方便用户配置 Caffe 的编译和安装而独立出来的，内容是一些变量，对这些变量进行赋值或取消部分注释可以修改编译和安装过程中的部分内容，具体的配置说明可参考 http://caffe.berkeleyvision.org/installation.html
# 取消注释可以开启 CUDNN 依赖库的使用，用于加入 Caffe 框架中的卷积计算加速
# USE_CUDNN := 1  # 可选

# 是否编译不含 GPU 依赖的 CPU 版本，如果不取消注释，则默认选择编译 GPU 版本
# CPU_ONLY := 1

# 是否使用这些数据读写方面的依赖库，默认都使用
# USE_OPENCV := 0
# USE_LEVELDB := 0
# USE_LMDB := 0

# 如果不会有同时读写 LMDB 数据的情况，可以取消该注释，去除代码中部分数据读写时的加锁保护，提高读写效率
# ALLOW_LMDB_NOLOCK := 1

# 如果使用的是 OpenCV 3 则需要取消该注释（该变量的存在）
# OPENCV_VERSION := 3

# 取消注释并修改赋值可以改变 cmake 默认的编译器选择
#   默认使用的编译器 Linux 平台为 g++，OSX 平台为 clang++
# CUSTOM_CXX := g++

# Caffe 中用到的 CUDA 依赖库配置，包含 CUDA 的 bin 目录 和 include 目录
CUDA_DIR := /usr/local/cuda
# 在 Ubuntu 14.04, CUDA 工具可以使用 "sudo apt-get install nvidia-cuda-toolkit" 安装，并取消下列注释
# CUDA_DIR := /usr

# CUDA 架构配置
# For CUDA < 6.0, comment the *_50 through *_61 lines for compatibility.
# For CUDA < 8.0, comment the *_60 and *_61 lines for compatibility.
# For CUDA >= 9.0, comment the *_20 and *_21 lines for compatibility.
CUDA_ARCH := -gencode arch=compute_20,code=sm_20 \
		-gencode arch=compute_20,code=sm_21 \
		-gencode arch=compute_30,code=sm_30 \
		-gencode arch=compute_35,code=sm_35 \
		-gencode arch=compute_50,code=sm_50 \
		-gencode arch=compute_52,code=sm_52 \
		-gencode arch=compute_60,code=sm_60 \
		-gencode arch=compute_61,code=sm_61 \
		-gencode arch=compute_61,code=compute_61

# BLAS 的值可以选择: atlas/mkl/open
# atlas 表示 ATLAS (默认)
# mkl 表示 MKL
# open 表示 OpenBLAS
BLAS := atlas
# 同时还需要指定 (MKL/ATLAS/OpenBLAS) 的头文件路径和链接库路径
# BLAS_INCLUDE := /path/to/your/blas
# BLAS_LIB := /path/to/your/blas

# 使用 Homebrew 搜索非标准安装的 openblas 依赖库路径
# BLAS_INCLUDE := $(shell brew --prefix openblas)/include
# BLAS_LIB := $(shell brew --prefix openblas)/lib

# 编译 MATLAB 接口，当然需要改成正确的路径
# MATLAB directory should contain the mex binary in /bin.
# MATLAB_DIR := /usr/local
# MATLAB_DIR := /Applications/MATLAB_R2012b.app

# 编译 Python 接口
# We need to be able to find Python.h and numpy/arrayobject.h.
PYTHON_INCLUDE := /usr/include/python2.7 \
		/usr/lib/python2.7/dist-packages/numpy/core/include
# 使用 Anacoda 中的 Python
# ANACONDA_HOME := $(HOME)/anaconda
# PYTHON_INCLUDE := $(ANACONDA_HOME)/include \
		# $(ANACONDA_HOME)/include/python2.7 \
		# $(ANACONDA_HOME)/lib/python2.7/site-packages/numpy/core/include

# 取消注释使用 Python 3 (default is Python 2)
# PYTHON_LIBRARIES := boost_python3 python3.5m
# PYTHON_INCLUDE := /usr/include/python3.5m \
#                 /usr/lib/python3.5/dist-packages/numpy/core/include

# 指定 Python 库所在路径 find libpythonX.X.so or .dylib.
PYTHON_LIB := /usr/lib
# PYTHON_LIB := $(ANACONDA_HOME)/lib

# 指定 Homebrew 查找非标准安装的 NumPy 库所在路径
# PYTHON_INCLUDE += $(dir $(shell python -c 'import numpy.core; print(numpy.core.__file__)'))/include
# PYTHON_LIB += $(shell brew --prefix numpy)/lib

# 打开注释将编译 Python 接口的网络层
# WITH_PYTHON_LAYER := 1

# 可以在 INLCUDE_DIRS 和 LIBRARY_DIRS 变量中加入自己的目录下的第三方库路径
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib

# 指定 Homebrew 的安装头文件和路径
# INCLUDE_DIRS += $(shell brew --prefix)/include
# LIBRARY_DIRS += $(shell brew --prefix)/lib

# NCCL 加速选择
# https://github.com/NVIDIA/nccl (last tested version: v1.2.3-1+cuda8.0)
# USE_NCCL := 1

# 取消注释将使用 `pkg-config` 工具查找 OpenCV 依赖库路径
# 如果在 $LIBRARY_DIRS 中指定了 OpenCV 的库路径，则 USE_PKG_CONFIG 没有效果
# USE_PKG_CONFIG := 1

# 执行 `make clean` 时删除的编译目录和发布目录
BUILD_DIR := build
DISTRIBUTE_DIR := distribute

# 取消注释后将以 DEBUG 模式编译程序 Does not work on OSX due to https://github.com/BVLC/caffe/issues/171
# DEBUG := 1

# 运行单元测试时使用的 GPU
TEST_GPUID := 0

# 关闭命令行的回显，作用等同于给 Q 赋值
Q ?= @

[NODE] @一般用来关闭命令的回显
如果 Q 取值为 @，那命令部分就是 @@:，不回显
如果 Q 没有取值，那命令部分就是 @:，一样不回显
[NODE] make 命令参数中提供 V=1 可以开启 V，V 是 Verbose 的缩写，打开了 V，所有的编译信息都将打印出来，关闭 V，将获得Beautify output。V 选项对于分析内核的编译很有帮助。而真正决定是否显示冗余回显的，是 V
```

下面接着 Makefile 中的内容分析

``` Makefile
# 确定编译目录
BUILD_DIR_LINK := $(BUILD_DIR)
ifeq ($(RELEASE_BUILD_DIR),)
	RELEASE_BUILD_DIR := .$(BUILD_DIR)_release
endif
ifeq ($(DEBUG_BUILD_DIR),)
	DEBUG_BUILD_DIR := .$(BUILD_DIR)_debug
endif

DEBUG ?= 0
ifeq ($(DEBUG), 1)
	BUILD_DIR := $(DEBUG_BUILD_DIR)
	OTHER_BUILD_DIR := $(RELEASE_BUILD_DIR)
else
	BUILD_DIR := $(RELEASE_BUILD_DIR)
	OTHER_BUILD_DIR := $(DEBUG_BUILD_DIR)
endif
```

``` Makefile
# 检索所有包含代码的文件的所在目录
## find * 列出当前目录及子目录下所有文件或目录的相对路径，-type d 只列出各级目录的相对路径
SRC_DIRS := $(shell find * -type d -exec bash -c "find {} -maxdepth 1 \
	\( -name '*.cpp' -o -name '*.proto' \) | grep -q ." \; -print)

# 定义动态链接库名字
LIBRARY_NAME := $(PROJECT)
LIB_BUILD_DIR := $(BUILD_DIR)/lib
STATIC_NAME := $(LIB_BUILD_DIR)/lib$(LIBRARY_NAME).a
DYNAMIC_VERSION_MAJOR 		:= 1  # 主版本号
DYNAMIC_VERSION_MINOR 		:= 0
DYNAMIC_VERSION_REVISION 	:= 0
DYNAMIC_NAME_SHORT := lib$(LIBRARY_NAME).so
#DYNAMIC_SONAME_SHORT := $(DYNAMIC_NAME_SHORT).$(DYNAMIC_VERSION_MAJOR)
# 定义带版本号的动态链接库名字
DYNAMIC_VERSIONED_NAME_SHORT := $(DYNAMIC_NAME_SHORT).$(DYNAMIC_VERSION_MAJOR).$(DYNAMIC_VERSION_MINOR).$(DYNAMIC_VERSION_REVISION)
DYNAMIC_NAME := $(LIB_BUILD_DIR)/$(DYNAMIC_VERSIONED_NAME_SHORT)
COMMON_FLAGS += -DCAFFE_VERSION=$(DYNAMIC_VERSION_MAJOR).$(DYNAMIC_VERSION_MINOR).$(DYNAMIC_VERSION_REVISION)
```

**Reference**

1. [跟我一起写Makefile](https://seisman.github.io/how-to-write-makefile/)
2. [Makefile中的wildcard用法](https://www.cnblogs.com/MMLoveMeMM/articles/3851812.html)

> TODO: 引用 1. 中的 doc 方式看起来很不错，先 mark，以后学习


``` Makefile
##############################
# 获取所有源代码文件
##############################
# CXX_SRCS 变量中包含所有除测试代码源文件的其他每个源文件的相对路径
## Caffe 框架的源代码存放在 src/caffe 目录下
CXX_SRCS := $(shell find src/$(PROJECT) ! -name "test_*.cpp" -name "*.cpp")  
# CU_SRCS 中包含 Caffe 框架 CUDA 版本源代码的相对路径
CU_SRCS := $(shell find src/$(PROJECT) ! -name "test_*.cu" -name "*.cu")
# TEST_SRCS 中为测试程序代码源文件路径
TEST_MAIN_SRC := src/$(PROJECT)/test/test_caffe_main.cpp     # 测试程序的主文件
TEST_SRCS := $(shell find src/$(PROJECT) -name "test_*.cpp") # 每个接口的测试文件
TEST_SRCS := $(filter-out $(TEST_MAIN_SRC), $(TEST_SRCS))    # 所有的测试文件
TEST_CU_SRCS := $(shell find src/$(PROJECT) -name "test_*.cu") # CUDA 接口的测试文件
GTEST_SRC := src/gtest/gtest-all.cpp  # gtest 测试文件，使用 Google gtest 库
# TOOL_SRCS 中包含一些基础工具的源代码文件，最终将独立便以为各个可执行文件
TOOL_SRCS := $(shell find tools -name "*.cpp")
# EXAMPLE_SRCS 中包含测试样例的源代码文件，最终将独立编译为各个可执行文件
EXAMPLE_SRCS := $(shell find examples -name "*.cpp")
# BUILD_INCLUDE_DIR 定义一个目录，该目录后面将包含所有编译中需要用到的头文件
BUILD_INCLUDE_DIR := $(BUILD_DIR)/src
# PROTO_SRCS 定义 proto 源文件目录
PROTO_SRC_DIR := src/$(PROJECT)/proto
PROTO_SRCS := $(wildcard $(PROTO_SRC_DIR)/*.proto)  # 将该文件目录的相对路径抓取出来
# PROTO_BUILD_DIR 编译过程的临时目录，存放 PROTO_SRCS 中的源文件文件，同时作为编译出的临时文件的路径
# PROTO_BUILD_INCLUDE_DIR 将包含 PROTO_SRCS 中源文件编译后生成的 .h 文件
PROTO_BUILD_DIR := $(BUILD_DIR)/$(PROTO_SRC_DIR)
PROTO_BUILD_INCLUDE_DIR := $(BUILD_INCLUDE_DIR)/$(PROJECT)/proto
# NONGEN_CXX_SRCS 除了生成的源文件（如 proto），包含其他所有的源文件和头文件，目的是为了后面对每个自己写的源文件中的代码依次进行代码风格检测
NONGEN_CXX_SRCS := $(shell find \
	src/$(PROJECT) \
	include/$(PROJECT) \
	python/$(PROJECT) \
	matlab/+$(PROJECT)/private \
	examples \
	tools \
	-name "*.cpp" -or -name "*.hpp" -or -name "*.cu" -or -name "*.cuh")
LINT_SCRIPT := scripts/cpp_lint.py     # 该文件为 cpp 代码检查工具
LINT_OUTPUT_DIR := $(BUILD_DIR)/.lint  # cpp_lint.py 的生成文件
LINT_EXT := lint.txt
LINT_OUTPUTS := $(addsuffix .$(LINT_EXT), $(addprefix $(LINT_OUTPUT_DIR)/, $(NONGEN_CXX_SRCS)))
EMPTY_LINT_REPORT := $(BUILD_DIR)/.$(LINT_EXT)
NONEMPTY_LINT_REPORT := $(BUILD_DIR)/$(LINT_EXT)
# PY$(PROJECT)_SRC python 的接口封装源文件
PY$(PROJECT)_SRC := python/$(PROJECT)/_$(PROJECT).cpp
PY$(PROJECT)_SO := python/$(PROJECT)/_$(PROJECT).so
PY$(PROJECT)_HXX := include/$(PROJECT)/layers/python_layer.hpp
# MAT$(PROJECT)_SRC mex 的编译文件
MAT$(PROJECT)_SRC := matlab/+$(PROJECT)/private/$(PROJECT)_.cpp
ifneq ($(MATLAB_DIR),)
	MAT_SO_EXT := $(shell $(MATLAB_DIR)/bin/mexext)
endif
MAT$(PROJECT)_SO := matlab/+$(PROJECT)/private/$(PROJECT)_.$(MAT_SO_EXT)
```

``` Makefile
##############################
# Derive generated files
##############################
# The generated files for protocol buffers
PROTO_GEN_HEADER_SRCS := $(addprefix $(PROTO_BUILD_DIR)/, \
		$(notdir ${PROTO_SRCS:.proto=.pb.h}))
PROTO_GEN_HEADER := $(addprefix $(PROTO_BUILD_INCLUDE_DIR)/, \
		$(notdir ${PROTO_SRCS:.proto=.pb.h}))
PROTO_GEN_CC := $(addprefix $(BUILD_DIR)/, ${PROTO_SRCS:.proto=.pb.cc})
PY_PROTO_BUILD_DIR := python/$(PROJECT)/proto
PY_PROTO_INIT := python/$(PROJECT)/proto/__init__.py
PROTO_GEN_PY := $(foreach file,${PROTO_SRCS:.proto=_pb2.py}, \
		$(PY_PROTO_BUILD_DIR)/$(notdir $(file)))
# The objects corresponding to the source files
# These objects will be linked into the final shared library, so we
# exclude the tool, example, and test objects.
CXX_OBJS := $(addprefix $(BUILD_DIR)/, ${CXX_SRCS:.cpp=.o})
CU_OBJS := $(addprefix $(BUILD_DIR)/cuda/, ${CU_SRCS:.cu=.o})
PROTO_OBJS := ${PROTO_GEN_CC:.cc=.o}
OBJS := $(PROTO_OBJS) $(CXX_OBJS) $(CU_OBJS)
# tool, example, and test objects
TOOL_OBJS := $(addprefix $(BUILD_DIR)/, ${TOOL_SRCS:.cpp=.o})
TOOL_BUILD_DIR := $(BUILD_DIR)/tools
TEST_CXX_BUILD_DIR := $(BUILD_DIR)/src/$(PROJECT)/test
TEST_CU_BUILD_DIR := $(BUILD_DIR)/cuda/src/$(PROJECT)/test
TEST_CXX_OBJS := $(addprefix $(BUILD_DIR)/, ${TEST_SRCS:.cpp=.o})
TEST_CU_OBJS := $(addprefix $(BUILD_DIR)/cuda/, ${TEST_CU_SRCS:.cu=.o})
TEST_OBJS := $(TEST_CXX_OBJS) $(TEST_CU_OBJS)
GTEST_OBJ := $(addprefix $(BUILD_DIR)/, ${GTEST_SRC:.cpp=.o})
EXAMPLE_OBJS := $(addprefix $(BUILD_DIR)/, ${EXAMPLE_SRCS:.cpp=.o})
# Output files for automatic dependency generation
DEPS := ${CXX_OBJS:.o=.d} ${CU_OBJS:.o=.d} ${TEST_CXX_OBJS:.o=.d} \
	${TEST_CU_OBJS:.o=.d} $(BUILD_DIR)/${MAT$(PROJECT)_SO:.$(MAT_SO_EXT)=.d}
# tool, example, and test bins
TOOL_BINS := ${TOOL_OBJS:.o=.bin}
EXAMPLE_BINS := ${EXAMPLE_OBJS:.o=.bin}
# symlinks to tool bins without the ".bin" extension
TOOL_BIN_LINKS := ${TOOL_BINS:.bin=}
# Put the test binaries in build/test for convenience.
TEST_BIN_DIR := $(BUILD_DIR)/test
TEST_CU_BINS := $(addsuffix .testbin,$(addprefix $(TEST_BIN_DIR)/, \
		$(foreach obj,$(TEST_CU_OBJS),$(basename $(notdir $(obj))))))
TEST_CXX_BINS := $(addsuffix .testbin,$(addprefix $(TEST_BIN_DIR)/, \
		$(foreach obj,$(TEST_CXX_OBJS),$(basename $(notdir $(obj))))))
TEST_BINS := $(TEST_CXX_BINS) $(TEST_CU_BINS)
# TEST_ALL_BIN is the test binary that links caffe dynamically.
TEST_ALL_BIN := $(TEST_BIN_DIR)/test_all.testbin
```

### 解读 Caffe CMakeLists.txt


### 解读 Caffe 头文件


### 解读 Caffe 内部实现


