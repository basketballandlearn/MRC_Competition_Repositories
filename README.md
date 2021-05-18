## 机器阅读理解(MRC)代码开源分享


****************************************************************************************** **更新** ******************************************************************************************
* 5/21：**开源基于大规模MRC数据预训练的模型**（包括roberta-wwm-large、macbert-large、albert-xxlarge）
* 5/18：开源代码


#### 仓库介绍
* 优化
  * 代码基于Hugginface的squad代码。之前自己开发，迭代版本多，而且很多细节没有考虑到（如：tok_to_orig、check_is_max_context以及空格、特殊字符、滑窗后答案的移动等处理），
    索性就直接在Hugginface的squad代码上迭代。 
    但是其实现的类缺乏对中文的支持，推理结果有一些影响，修改之后此库能较好的支持中文，抽取的答案精度也尽可能不受影响。
* 由来
  * 此仓库原本是我2019年参加百度举办的Dureader比赛的代码分享。由于BERT刚出来不久，预训练模型还没普及，用的都是BiDAF、QANet等。torch也没搞懂是啥，只觉得BERT名气挺大的，就用了，混了个第七，也蹭了挺多start，搭了一波顺风车 嘿嘿😁！从此便开启了调参之旅😁！
* 目的
  * 此仓库是我这几年（2019-2021）参加各种中文机器阅读理解（MRC）比赛的开源分享
    * 现在比赛都被大佬们拿下，很多方法细节没有公开，导致"散户"的我上分困难😰。所以这个库旨在为大家提供一个效果不错的**强基线**，大家可以在这基础上改进，也可以借鉴融合进自己的方法。
    * 有些由于"年代久远"也整理不过来了（others文件夹下的比赛）。不过方案和代码都有，对比着看很最容易就看懂了


### 环境
```
pip install transformers==2.10.0 
```


## 小小提示：
* 代码上传前已经跑通。文件不多，所以如果碰到报错之类的信息，可能是代码路径不对、缺少安装包等问题，一步步解决，可以提issue
* 拿现有的预训练模型基于这套代码微调可以有一个很高的基线，已有小伙伴在Dureader比赛中取得很高的成绩


### 运行流程  
###### 一、数据 & 模型：
* 将train、dev、test等数据放在datasets文件夹下 ([格式符合squad的就行](https://aistudio.baidu.com/aistudio/competition/detail/66))
* 指定模型目录
###### 二、一键运行
```
cd main
sh train_bert.sh
(包括训练/验证/预测：--do_train、--evaluate_during_training、--do_test)
```
###### 三、无答案问题
* 如果包含无答案类型数据，加入--version_2_with_negative就行
* 将数据替换为Dureader2021_checklist的数据可以直接跑(已有多位小伙伴在2021_checklist数据集上测试了效果，大概处于F1 65这样)


#### 比赛

* **Dureader checklist 2021语言与智能技术竞赛**（目前第一）
* **疫情政务问答助手**（第一）
* **Dureader robust 2020语言与智能技术竞赛**（第二）
* **成语阅读理解**（第二）
* **莱斯杯**（第三）
* **Dureader 2019语言与智能技术竞赛**（第七）


#### 基于大规模MRC数据预训练的模型
* 数据来源（百万规模）
  网上收集的大量中文MRC数据
  （其中包括 Dureader、WebQA、SogoQA、Squad-2.0、CMRC-2018、法研杯、军事阅读理解、EpidemicQA以及自己爬取的网页数据等，
  这里面包括了百度百科、搜狗搜索、军事、法律、医疗、教育等领域。）
* 然后用此库代码进行MRC任务的预训练（将得到的模型放到下游任务微调可比直接使用预训练语言模型提高4个点以上（Dureader上的的实验结果，其他数据集还没具体测试））
