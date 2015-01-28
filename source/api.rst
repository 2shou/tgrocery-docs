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
      * 文件路径：一行为一个训练样本，类别标签在前、语料文本在后，默认分隔符是\ ``\t``

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
