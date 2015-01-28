.. TextGrocery documentation master file, created by
   sphinx-quickstart on Wed Jan 28 11:34:57 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

欢迎使用TextGrocery
===================

TextGrocery是一个基于
`LibLinear <http://www.csie.ntu.edu.tw/~cjlin/liblinear>`_
和
`结巴分词 <https://github.com/fxsjy/jieba>`_
的短文本分类工具，特点是高效易用，同时支持中文和英文语料。

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

1. 通过
   `GitHub <https://github.com/2shou/TextGrocery>`_
   （版本更新）

.. code:: bash

    git clone https://github.com/2shou/TextGrocery.git
    cd TextGrocery
    make

2. 通过
   `pip <https://pypi.python.org/pypi?:action=display&name=tgrocery>`_
   （更稳定）

.. code:: bash

    pip install tgrocery

快速开始
=======

.. code:: python

    >>> from tgrocery import Grocery
    # 新开张一个杂货铺（别忘了取名）
    >>> grocery = Grocery('sample')
    # 训练文本可以用列表传入
    >>> train_src = [
        ('education', '名师指导托福语法技巧：名词的复数形式'),
        ('education', '中国高考成绩海外认可 是“狼来了”吗？'),
        ('sports', '图文：法网孟菲尔斯苦战进16强 孟菲尔斯怒吼'),
        ('sports', '四川丹棱举行全国长距登山挑战赛 近万人参与')
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
    >>> new_grocery.predict('考生必读：新托福写作考试评分标准')
    education
    # 测试
    >>> test_src = [
        ('education', '福建春季公务员考试报名18日截止 2月6日考试'),
        ('sports', '意甲首轮补赛交战记录:米兰客场8战不败国米10年连胜'),
    ]
    >>> new_grocery.test(test_src)
    # 输出测试的准确率
    0.5
    # 同样可支持文件传入
    >>> new_grocery.test('test_ch.txt')
    # 自定义分词模块（必须是一个函数）
    >>> custom_grocery = Grocery('custom', custom_tokenize=list)

API文档
=======

Grocery
-------

class tgrocery.Grocery(name, custom_tokenize=None)
  * 确定你的分类项目名
  * custom_tokenize会覆盖默认的分词单元（结巴分词），要求custom_tokenize的类型必须是函数

def Grocery.train(train_src, delimiter='\t')
  输入训练样本，生成分类模型

  * train_src可以是嵌套列表或文件路径

      * 嵌套列表：实体是两个字符串构成的tuple，第一个字符串是类别标签，第二个字符串是语料文本
      * 文件路径：一行为一个训练样本，类别标签在前、语料文本在后，默认分隔符是``\t``
        
  * delimiter仅在train_src为文件路径时生效，可以自定义文件的分隔符
