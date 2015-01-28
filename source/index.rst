.. TextGrocery documentation master file, created by
   sphinx-quickstart on Wed Jan 28 11:34:57 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

欢迎使用TextGrocery
===================

TextGrocery是一个基于
`LibLinear <http://www.csie.ntu.edu.tw/~cjlin/liblinear>`_
的短文本分类工具，特点是高效易用。

TextGrocery整合了
`结巴分词 <https://github.com/fxsjy/jieba>`_
作为默认的分词单元，以支持中文的短文本分类。

性能
====



========================  =============  ===============
分类器                      准确率（%）   		计算时间（秒）
========================  =============  ===============
scikit-learn(朴素贝叶斯)  	76.8%           134
scikit-learn(svm)         	76.9%           121
**TextGrocery**           **79.6%**       **49**
========================  =============  ===============
