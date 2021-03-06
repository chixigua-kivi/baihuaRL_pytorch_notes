# DRLPytorch-
《白话强化学习与PyTorch》的学习笔记

代码参考https://github.com/GAOYANGAU/DRLPytorch

# 第五章-时间差分
## 5.1 Qlearning.py
对源代码（native-Qlearning.py）结构进行了更改，主要体现在如下2个方面：
1. 随机产生初始点后，采用epsilon-贪婪发进行动作选择，当到达目标状态后为一个Episode
2. 动作价值更新公式为完整的Q计算公式（原代码将alpha取为了1）

# 第六章 深度学习

总结：https://blog.csdn.net/cat_ziyan/article/details/101344278 

## 6.1 linear_regression.py 
对书中77页代码进行修改，使其适应0.4以后的版本，主要有一下3个部分
1. 无需再import torch.autograd.Variable，Tensor直接可以计算梯度
2. 累加损失时.data[0]改为.item()，0.4以后的版本中loss是一个零维的标量，用loss.item()可以从标量中获取Python数字。
3. 增加了对模型保存和加载，快速搭建神经网络的学习笔记

 
## 6.2 feedforward_neural_net.py
对书中119页代码修改为GPU版本，添加了GPUtil等几个小模块，实时监测GPU信息；
分别在CPU和GPU下运行后：CPU用时80s，GPU用时37s。

## 6.3 convoluntional_neural_network.py
对书中125页代码的学习过程中，将其修改为GPU版本后出现了如下错误:

![image](https://github.com/catziyan/DRLPytorch-/blob/master/erro.png)

考虑到在使用全连接网络时可以使用GPU，故放弃思考（繁琐的）cuda，cudnn的本版问题，直接使用torch.backends.cudnn.enabled = False 解决问题。
但具体为何造成此错误，作为小蚂蚁的我不得而知

与全连接神经网络相比，卷积神经网络的准确率更高（全连接97%、96%左右，卷积神经网络99%），但速度更慢，共用了105s，GPU的占用率也越高，用GPUtil模块打印结果如图：（之所以放图是因为刚学了如何在github插入图片，haha）

![image](https://github.com/catziyan/DRLPytorch-/blob/master/GPU.png)

## 6.4 Recurrent_Neural_net.py
对书中143页代码学习过程中，研究了两个细节问题：
1. 在计算正确率时，用（100*(correct/total)）计算得到结果的总是0。 因为correct是由tensor计算得到的，故correct也为tensor，且数据类型为torch.int64。 在pytorch中的int/long之间的运算得到的还是整形，故计算结果总为0.
2. 书中对out = self.fc(out[:,-1,:])分析为“这是一个二维张量，第一个维度是batch_size，第二个维度是input_size，尺寸为[100,28]”，而全连接网络的输入为elf.fc = torch.nn.Linear(hidden_size, output_size)，则显然out[:,-1,:]的尺寸应该为[batch_size, hidden_size]=[100,36].

# 第八章 DQN算法族
第八章用了nips-DQN、nature-DQN、double-DQN、dueling DQN四种算法来训练Gym中的atari游戏—pong，相关代码在08文件夹中
相关总结：https://blog.csdn.net/cat_ziyan/article/details/101712107

# 第九章 PG算法族
https://blog.csdn.net/cat_ziyan/article/details/103903041
