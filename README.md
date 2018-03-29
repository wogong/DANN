# DANN

**Theano** version code of following papers.

    @InProceedings{icml2015-ganin15,
    Title                    = {Unsupervised Domain Adaptation by Backpropagation},
    Author                   = {Yaroslav Ganin and Victor Lempitsky},
    Booktitle                = {Proceedings of the 32nd International Conference on Machine Learning},
    Year                     = {2015},

    Address                  = {Lille, France},
    Editor                   = {Francis Bach and David Blei},
    Month                    = {07--09 Jul},
    Pages                    = {1180--1189},
    Publisher                = {PMLR},
    Series                   = {Proceedings of Machine Learning Research},
    Volume                   = {37},
    Url                      = {http://proceedings.mlr.press/v37/ganin15.html}
    }

    @Article{ganin2016domain,
    Title                    = {Domain-adversarial training of neural networks},
    Author                   = {Ganin, Yaroslav and Ustinova, Evgeniya and Ajakan, Hana and Germain, Pascal and Larochelle, Hugo and Laviolette, Fran{\c{c}}ois and Marchand, Mario and Lempitsky, Victor},
    Journal                  = {Journal of Machine Learning Research},
    Year                     = {2016},
    Number                   = {59},
    Pages                    = {1--35},
    Volume                   = {17}
    }

## Details

### run

项目文件下只有四个文件夹：SRC，data，result

SRC存储了项目所有的源代码：
运行顺序：generate_data.py -> generateMNIST_SandMNIST_T.py -> cnn_~~~.py

generate_data.py:根据mnist.pkl.gz生成目标域数据集：target~.pkl和st~.pkl（存在data/下）
可以在generate_data.py中修改theta（源域和目标域图片做差时乘的比例，防止两个域的差距过大）

generateMNIST_SandMNIST_T.py：根据source.pkl、target~.pkl和st~.pkl生成部分图片，用来做展示
只生成了一部分，train,valid,test各生成10个图片。

cnn_ts_ts.py:在源域上训练，在源域上测试

cnn_ts_tt.py:在源域上训练，在目标域上测试（验证集为目标域数据）

cnn_tt_tt.py:在目标域上训练，在目标域上测试

cnn_tst_ts.py:在源域和目标域上训练，在目标域上测试（也就是使用领域自适应机制）（验证集合为目标域数据）
可以在其中调节lmbda（论文中公式4的lambda）

上述4个文件将结果输出在txt文件中，存在result/下

### data preparation

data下存了项目需要存的数据：
在运行程序前要具有以下文件：BSR文件夹，imageMNIST_T文件夹，imageMNIST_S文件夹，mnist.pkl.gz, source.pkl

其中BSR文件夹：
下载来源：<http://www.eecs.berkeley.edu/Research/Projects/CS/vision/grouping/BSR/BSR_bsds500.tgz>
数据集介绍：<https://www2.eecs.berkeley.edu/Research/Projects/CS/vision/grouping/resources.html#bsds500>
下载完成之后将BSR_bsds500.tgz解压，包含一个BSR文件夹，把这个BSR文件夹放到data下就行

imageMNIST_S和imageMNIST_T:两个文件夹存储generateMNIST_SandMNIST_T.py生成的图片，
imageMNIST_S存储源域的图片
imageMNIST_T存储目标域图片
运行前为空即可
imageMNIST_S和imageMNIST_T中名字相同的图片相对应

mnist.pkl.gz：是mnist数据集，运行前要下载好
数据来源：<http://www.iro.umontreal.ca/~lisa/deep/data/mnist/mnist.pkl.gz>

source.pkl：是mnist.pkl.gz的解压文件，运行前要解压好

运行后会生成st~.pkl和target~.pkl存在data下：
其中~部分表示theta的取值

### results

`result`下存了运行结果，运行结果文件命名方式和代码文件命名方式相同

### hyper parameters

1. theta：源域和噪声域图片同像素点灰度值做差时，噪声域乘的比例。
    论文中4.1Results--MNIST->MNIST-M中的图片生成公式，我在I1和I2做差时对I2乘了一个theta，控制源域和目标域的差距。
    theta在0到1之间，越小差距越小，越大差距越大

2. lmbda：论文中公式4的lambda。控制在反向传播过程中，域分类器的损失对特征提取器的参数的修改程度。
    lmbda在0到1之间，lmbda越小，域分类器的损失对特征提取器的参数的修改程度越小。

## Reference

- CNN代码解读博文：<http://blog.csdn.net/u012162613/article/details/43225445>
- CNN代码来源：<https://github.com/wepe/MachineLearning/blob/master/DeepLearning%20Tutorials/cnn_LeNet/convolutional_mlp_commentate.py>
