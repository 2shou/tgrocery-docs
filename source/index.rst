.. TextGrocery documentation master file, created by
   sphinx-quickstart on Wed Jan 28 11:34:57 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

欢迎使用TextGrocery
===================

TextGrocery是一个基于\ `LibLinear <http://www.csie.ntu.edu.tw/~cjlin/liblinear>`_\ 和\ `结巴分词 <https://github.com/fxsjy/jieba>`_\ 的短文本分类工具，特点是高效易用，同时支持中文和英文语料。

`GitHub传送门 <https://github.com/2shou/TextGrocery>`_

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
  获取训练样本，生成分类模型

  * train_src可以是嵌套列表或文件路径

      * 嵌套列表：实体是两个字符串构成的tuple，第一个字符串是类别标签，第二个字符串是语料文本
      * 文件路径：一行为一个训练样本，类别标签在前、语料文本在后，默认分隔符是\ ``\\t``

  * delimiter是解析训练样本时所用的分隔符，仅在train_src为文件路径时生效

def Grocery.get_load_status()
  返回目前模型是否在已训练或已加载的状态

def Grocery.predict(single_text)
  * 对单一文本预测其类别（预测前会检测模型是否已训练或已加载）
  * 返回一个\ ``GroceryPredictResult``\ 对象

def Grocery.save()
  保存模型到本地

  * 默认文件夹名是Grocery的name属性
  * 如果本地存在同名文件夹，将被覆盖

def Grocery.load()
  从本地加载模型

  * 默认文件夹名是Grocery的name属性
  * 分词单元的信息不会被自动加载，如果自定义了分词单元，需要在创建Grocery的过程中再次指定

def Grocery.test(test_src, delimiter='\t')
  测试模型在测试样本中取得的准确率

  * test_src可以是嵌套列表或文件路径

    * 嵌套列表：实体是两个字符串构成的tuple，第一个字符串是类别标签，第二个字符串是语料文本
    * 文件路径：一行为一个测试样本，类别标签在前、语料文本在后，默认分隔符是\ ``\\t``
  
  * delimiter是解析测试样本时所用的分隔符，仅在test_src为文件路径时生效
  * 返回一个\ ``GroceryTestResult``\ 对象

GroceryPredictResult
--------------------

对新语料预测后的结果

GroceryPredictResult.predicted_y
  预测的类别标签

GroceryPredictResult.dec_values
  * 对所有类别的决策变量（一个浮点数，可正可负，越大表示归属于该类别的可能性越大）
  * dict，key是类别标签，value是决策变量

GroceryTestResult
------------------

对测试样本测试后的结果

GroceryTestResult.accuracy_overall
  不分类别的总体准确率，浮点数，0到1之间

GroceryTestResult.accuracy_labels
  * 区分类别的准确率
  * dict，key是类别标签，value是准确率

GroceryTestResult.recall_labels
  * 区分类别的召回率
  * dict，key是类别标签，value是召回率

def GroceryTestResult.show_result()
  * 打印各类别的准确率和召回率表格，方便比较
