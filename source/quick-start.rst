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
