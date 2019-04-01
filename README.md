### 环境

System: Manjaro 18.0.2, Linux core 4.19.13

Environment: Anaconda  python3.6 jupyter-notebook

Using Packages:
    tensorflow r1.12-gpu
    numpy 1.15
    pickle
    random

### 代码说明

论文：[Content-Oriented User Modeling for Personalized Response Ranking in Chatbots](https://ieeexplore.ieee.org/document/8068225/)

论文目标：让聊天机器人为特定用户选择特定回复，以达成个性化回复的目的

论文中需要实现三个模型，分别为：

 	1. Seq2Seq模型：
      	1. 无attention的Encoder-Decoder结构，用于将句子编码成一个向量
      	2. 训练：一个句子，用Encoder编码成一个向量，用Decoder解码，目标句子为同一个句子
      	3. 使用：最终只使用Encoder编码的句子向量
 	2. User Modeling模型：
      	1. 多层感知机结构，用于对用户建模，每个用户训练一个对应的embedding
      	2. 训练：句子 + 随机初始化用户embedding 进行二分类，句子可能为用户过去发布的内容，可能为随机抽取的
      	3. 使用：最终更新完毕的用户embedding
 	3. Ranking 模型：
      	1. 多层感知机结构，根据用户embedding和当前询问给回复打分
      	2. 训练：详见论文
      	3. 使用：最终的sigmoid分数为置信度，十个候选回复中置信度最高的为最终选择

### 文件说明：

文件：

1. jupyter notebook 文件：
   1. 00、01、02 为数据准备，分别为三个模型构建tf record
   2. 10、11、12 为模型训练和验证，分别为Seq2Seq模型、User模型和	Ranking模型
      	
2. py 文件：
   1. preppy 为数据准备类，获取原始数据转换成tf record
   2. processText 为预处理脚本，随机生成文本
   3. rank、seq2seq、user 分别定义了如文件名所示的模型
3. 文件夹：
   1. Data：包含三个模型所使用的文本数据、词汇表、生成的tf record
   2. Model：包含三个模型训练后的参数保存