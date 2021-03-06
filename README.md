# Text-Classification-Benchmark
文本分类基准测试

#### 测试分类器
1. 贝叶斯

2. 逻辑回归

3. 线性 SVM

4. 非线性 SVM(RBF)

5. 随机森林

6. XGBoost

7. LightGBM
#### 语料
文件名: FDU_NLP_corpus_seg_balanced.txt

描述: 不同领域的新闻、文献等 (中文)

格式: 已经分词, 每一行对应一篇文本. 具体格式如下
> {分类名}@{文本}
>
> {分类名}@{文本}
>
> ...

规模: 共 4050 篇(平衡语料)

类别: 共 9 个类别, 分别为: Art, Enviornment, Space, Sports, Computer, Politics, Economy, Agriculture, History.

来源: 复旦大学计算机信息与技术系国际数据库中心自然语言处理小组

#### 特征处理
1. 卡方校验(chi-square test) 进行特征选择, 共选择 1000 个特征词作为特征.
2. 通过 TF-IDF 进行特征提取(向量化)

#### 基准测试
基于 scikit-learn 自带模型的**默认参数**进行"5次交叉验证(cross validation)"

#### 参考结果
不同算法模型对超参数调优存在差异, 以下结果仅供参考:

**基于原始 TF-IDF 特征**
<pre>
+------------+----------+------------+------------------------+--------------------+---------------+
| classifier | fit_time | score_time |  test_precision_micro  | test_recall_micro  | test_f1_micro |
+------------+----------+------------+------------------------+--------------------+---------------+
|     NB     |  0.008   |   0.005    |         0.865          |       0.865        |     0.865     |
+------------+----------+------------+------------------------+--------------------+---------------+
|     LR     |  0.312   |   0.004    |         0.903          |       0.903        |     0.903     |
+------------+----------+------------+------------------------+--------------------+---------------+
|   L-SVM    |  0.124   |   0.004    |          0.91          |        0.91        |     0.91      |
+------------+----------+------------+------------------------+--------------------+---------------+
|  RBF-SVM   |  14.824  |   6.469    |         0.825          |       0.825        |     0.825     |
+------------+----------+------------+------------------------+--------------------+---------------+
|     RF     |  3.277   |   0.092    |         0.922          |       0.922        |     0.922     |
+------------+----------+------------+------------------------+--------------------+---------------+
|    XGB     |  32.498  |   0.169    |         0.938          |       0.938        |     0.938     |
+------------+----------+------------+------------------------+--------------------+---------------+
|    LGBM    |  37.79   |   0.162    |         0.942          |       0.942        |     0.942     |
+------------+----------+------------+------------------------+--------------------+---------------+
</pre>

**基于标准化(保留均值) TF-IDF 特征**

备注: 不涉及中心化,原特征矩阵的稀疏性被保留.
> StandardScaler(with_mean=False, with_std=True)
<pre>
+------------+----------+------------+------------------------+--------------------+---------------+
| classifier | fit_time | score_time |  test_precision_micro  | test_recall_micro  | test_f1_micro |
+------------+----------+------------+------------------------+--------------------+---------------+
|     NB     |  0.022   |   0.008    |          0.86          |        0.86        |     0.86      |
+------------+----------+------------+------------------------+--------------------+---------------+
|     LR     |  1.154   |   0.006    |         0.894          |       0.894        |     0.894     |
+------------+----------+------------+------------------------+--------------------+---------------+
|   L-SVM    |  1.107   |   0.006    |         0.875          |       0.875        |     0.875     |
+------------+----------+------------+------------------------+--------------------+---------------+
|  RBF-SVM   |  10.972  |    6.79    |         0.896          |       0.896        |     0.896     |
+------------+----------+------------+------------------------+--------------------+---------------+
|     RF     |  1.997   |   0.073    |         0.921          |       0.921        |     0.921     |
+------------+----------+------------+------------------------+--------------------+---------------+
|    XGB     |  75.364  |   0.097    |         0.937          |       0.937        |     0.937     |
+------------+----------+------------+------------------------+--------------------+---------------+
|    LGBM    |  45.986  |   0.182    |         0.942          |       0.942        |     0.942     |
+------------+----------+------------+------------------------+--------------------+---------------+
</pre>

**基于标准化 TF-IDF 特征**

备注: 涉及中心化,原特征矩阵的稀疏性已改变,实际上是一个稠密矩阵. 中心化引入负值特征, 故不进行贝叶斯测试.
> StandardScaler(with_mean=True, with_std=True)
<pre>
+------------+----------+------------+------------------------+--------------------+---------------+
| classifier | fit_time | score_time |  test_precision_micro  | test_recall_micro  | test_f1_micro |
+------------+----------+------------+------------------------+--------------------+---------------+
|     LR     |  10.084  |   0.006    |         0.888          |       0.888        |     0.888     |
+------------+----------+------------+------------------------+--------------------+---------------+
|   L-SVM    |  17.493  |   0.006    |         0.867          |       0.867        |     0.867     |
+------------+----------+------------+------------------------+--------------------+---------------+
|  RBF-SVM   |  9.889   |   6.029    |         0.896          |       0.896        |     0.896     |
+------------+----------+------------+------------------------+--------------------+---------------+
|     RF     |  1.897   |   0.074    |         0.921          |       0.921        |     0.921     |
+------------+----------+------------+------------------------+--------------------+---------------+
|    XGB     |  75.652  |   0.102    |         0.937          |       0.937        |     0.937     |
+------------+----------+------------+------------------------+--------------------+---------------+
|    LGBM    |  49.342  |   0.169    |         0.944          |       0.944        |     0.944     |
+------------+----------+------------+------------------------+--------------------+---------------+
</pre>
# MIT License

Copyright (c) 2018 FelixHo

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.