.. TextGrocery documentation master file, created by
   sphinx-quickstart on Wed Jan 28 11:34:57 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

欢迎使用TextGrocery
===================

TextGrocery是一个基于\ `LibLinear <http://www.csie.ntu.edu.tw/~cjlin/liblinear>`_\ 和\ `结巴分词 <https://github.com/fxsjy/jieba>`_\ 的短文本分类工具，特点是高效易用，同时支持中文和英文语料。

`GitHub项目链接 <https://github.com/2shou/TextGrocery>`_

性能
====

* 训练集：来自32个类别的4.8万条中文新闻标题
* 测试集：来自32个类别的1.6万条中文新闻标题
* 与scikit-learn的svm和朴素贝叶斯算法做横向对比

========================  =============  ===============
分类器                      准确率（%）           计算时间（秒）
========================  =============  ===============
scikit-learn(朴素贝叶斯)      76.8%           134
scikit-learn(svm)             76.9%           121
**TextGrocery**           **79.6%**       **49**
========================  =============  ===============

安装
====

1. 通过\ `GitHub <https://github.com/2shou/TextGrocery>`_\ （最新版本）

.. code:: bash

    git clone https://github.com/2shou/TextGrocery.git
    cd TextGrocery
    make

2. 通过\ `pip <https://pypi.python.org/pypi?:action=display&name=tgrocery>`_\ （更稳定）

.. code:: bash

    pip install tgrocery

使用
====

.. toctree::
    :maxdepth: 2

    quick-start
    api
