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

* 训练集：来自32个类别的4.8万条中文新闻标题
* 测试集：来自32个类别的1.6万条中文新闻标题
* 与scikit-learn的svm和朴素贝叶斯算法做横向对比

========================  =============  ===============
分类器                      准确率（%）   		计算时间（秒）
========================  =============  ===============
scikit-learn(朴素贝叶斯)  	76.8%           134
scikit-learn(svm)         	76.9%           121
**TextGrocery**           **79.6%**       **49**
========================  =============  ===============

快速开始
=======

.. code:: python

    >>> from tgrocery import Grocery
    # 新开张一个杂货铺（别忘了取名）
    >>> grocery = Grocery('sample')
    # 训练文本可以用列表传入
    >>> train_src = [
        ('education', 'Student debt to cost Britain billions within decades'),
        ('education', 'Chinese education for TV experiment'),
        ('sports', 'Middle East and Asia boost investment in top level sports'),
        ('sports', 'Summit Series look launches HBO Canada sports doc series: Mudhar')
    ]
    >>> grocery.train(train_src)
    # 也可以用文件传入（默认以tab为分隔符，也支持自定义）
    >>> grocery.train('train_ch.txt')
    # 保存模型
    >>> grocery.save()
    # 加载模型（名字和保存的一样）
    >>> new_grocery = Grocery('sample')
    >>> new_grocery.load()
    # 预测
    >>> new_grocery.predict('Abbott government spends $8 million on higher education media blitz')
    education
    # 测试
    >>> test_src = [
        ('education', 'Abbott government spends $8 million on higher education media blitz'),
        ('sports', 'Middle East and Asia boost investment in top level sports'),
    ]
    >>> new_grocery.test(test_src)
    # 输出准确率
    1.0
    # 同样可支持文件传入
    >>> new_grocery.test('test_ch.txt')
    # 自定义分词模块（必须是一个函数）
    >>> custom_grocery = Grocery('custom', custom_tokenize=list)