# 1. LeNet-5  tensorflow implementation
### network structure
![image](https://user-images.githubusercontent.com/53073447/151538573-46fd140b-6edf-40ea-aec5-392a1b26a683.png)

输入层
- 28*28的图片，也就是相当于784个神经元

C1层(卷积层)
- 选择6个5*5的卷积核，得到6个大小为24*24（28-5+1=24）的特征图
- 参数个数：（5*5+1）*6=156
- 连接个数：（5*5+1）*6*24*24=89,856

S2层(下采样层)
- 每个下抽样节点的4个输入节点求和后取平均(平均池化)，均值乘上一个权重参数加上一个偏置参数作为激活函数的输入，激活函数的输出即是下一层节点的值。池化核大小选择2*2，得到6个12*12大小特征图

C3层(卷积层)
- 用16个5*5的卷积核对S2层输出的特征图进行卷积后，得到16张8*8（12-5+1=8）的特征图
- 参数个数：（5*5+1）*16=416
- 连接个数：（5*5+1）*16*8*8=26,624

S4层(下采样层)
- 对C3的16张8*8特征图进行最大池化，池化核大小为2*2，得到16张大小为4*4的特征图。神经元个数已经减少为:16*4*4=256

C5层(扁平层)
- Flatten层用来将输入“压平”，即把多维的输入一维化，常用在从卷积层到全连接层的过渡。

F6层(全连接层)
- 有500个节点，该层的训练参数和连接数都是(256+1)x500=128,500

Output层
- 共有10个节点，分别代表数字0到9，如果节点 i 的输出值为0，则网络识别的结果是数字 i


### Loss calculation for one data
10分类，假设第一个样本(7) onehot编码是[0,0,0,0,0,0,0,1,0,0]
LeNet-5分类predict第一个样本结果是[8.5153336e-11, 2.7740224e-10, 9.3247691e-06, 3.6015678e-07, 4.3397272e-11, 6.4643867e-11, 1.9824928e-14, 9.9999022e-01, 3.8150852e-10, 8.1198380e-08]

loss计算用交叉熵，第1个样本的loss计算如下：
1、找出onehot编码数组中最大的值，如上：9.9999022e-01
2、-1 * （log（9.9999022e-01））， 1：真实编码为7的概率为1；9.9999022e-01：预测为7的概率值。
