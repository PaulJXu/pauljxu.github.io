---
layout: post
title: python编程导论notes4
subtitle: program use python notes4
author: jerpson
tags:
    - python
---

## 16 蒙特卡罗模拟

蒙特卡罗模拟用于求事件的近似概率，它**多次执行同一模拟**，然后**将结果进行平均**。

### 16.1 帕斯卡的问题

“连续掷一对骰子24次得到两个6”这个赌注是否有利可图？

1、第一次投掷时，每个骰子掷出6的概率是1/6，所以两个骰子都掷出6的概率是1/36；
2、因此，第一次投掷时没有掷出两个6的概率是1 – 1/36 = 35/36；
3、因此，连续24次投掷都没有掷出两个6的概率是(35/36) 24 ，差不多是0.51，所以掷出两个6
的概率是1 – (35/36) ^24^ 。大约是0.49。长期来看，在24次投掷中掷出两个6这个赌注是无利可图的。

```python
#验证帕斯卡的分析
def rollDie():
    return random.choice([1,2,3,4,5,6])

def checkPascal(numTrials):
    '''假设numTrials是正整数
    输出获胜概率的估值'''
    numWins = 0
    for i in range(numTrials):
        for j in range(24):
            d1 = rollDie()
            d2 = rollDie()
            if d1 == 6 and d2 == 6:
                numWins += 1
                break
    print('Probability of winning =', numWins/numTrials)
```

### 16.2 过线还是不过线

```python
class CrapsGame(object):
    def __init__(self):
        self.passWins, self.passLosses = 0, 0
        self.dpWins, self.dpLosses, self.dpPushes = 0, 0, 0
     
    def playHand(self):
        throw = rollDie() + rollDie()
        if throw == 7 or throw == 11:
            self.passWins += 1
            self.dpLosses += 1
        elif throw ==2 or throw == 3 or throw == 12:
            self.passLosses += 1
        	if throw == 12:
                self.dpPushes += 1
            else:
                self.dpWins += 1
        else:
            point = throw
            while True:
                throw = rollDie() + rollDie()
                if throw == point:
                    self.passWins += 1
                    self.dpLosses += 1
                    break
                elif throw == 7:
                    self.passLosses += 1
                    self.dpWins += 1
                    break
                   
 	def passResults(self):
        return (self.passWins, self.passLosses)
    
    def dpResult(self):
        return(self.dpWins, self.dpLosses, self.dpPushes)
```

CrapsGame 类的实例变量会记录游戏开始后过线和不过线的情况。观察者方法 passResult 和dpResults 可以返回两种选择中胜利、失败或平局的次数。 playHand 方法可以模拟一手游戏，当掷手掷出“出场掷”时，就开始新的一“手”，在双骰儿游戏中，“出场掷”是指点数出现之前的那次投掷。当掷手赢得或输掉自己的初始赌注时，一手结束。 playHand 方法中的主体代码就是对上述规则的算法描述。请注意， else 从句中有一个循环，对应出现点数的情况。当掷出7或者点数的时候，使用 bread 语句跳出循环。

### 16.4 求Π的值

```python
def throwNeedles(numNeedles):
    inCircle = 0
    for Needles in range(1, numNeedles+1):
        x = random.random()#0-1之间的随机数
        y = random.random()
        if (x*x + y*y)**0.5 <= 1:
            inCircle += 1
    #数出一个1/4圆中的针数，乘以4得到整个圆中的针数量
    return 4*(inCircle/numNeedles)

def getEst(numNeedles, numTrials):
    estimates = []
	for t in range(numTrials):
        piGuess = throwNeedles(numNeedles)
        estimates.append(piGuess)
    sDev = stdDev(estimates)
    curEst = sum(estimates)/len(estimates)
    print('Est. =', str(round(curEst, 5))+',','Std. dev. =', str(round(sDev, 5))+',','Needles = ', numNeedles)
    return(curEst, sDev)

def estPi(precision, nunTrials):
    numNeedles = 1000
    sDev = precision
    while sDev > precision/1.96:
        curEst, sDev = getEst(numNeedles, numTrials)
        numNeedles *= 2
    return curEst
```

函数 throwNeedles 模拟了扔针的过程。首先，使用 random.random 得到一对正的笛卡儿坐标值（x值和y值），表示相对于正方形中心点的针的位置。然后，使用勾股定理计算出底为x高为y的直角三角形的斜边的长度，这就是针尖与原点（正方形中心点）之间的距离。因为圆的半径是1 ，所以当且仅当针尖与原点之间的距离不大于 1 时，针才落在圆内。我们就根据这个事实数出落在圆内的针的数量。
函数 getEst 使用 throwNeedles 找出 π 的估计值。首先扔出 numNeedles 根针，然后取numTrials 次实验结果的平均值，最后返回整个实验的均值和标准差。函数 estPi 使用不断增加的针的数量来调用 getEst ，直到 getEst 返回的标准差不大于precision/1.96 。基于误差服从正态分布的假设，这意味着 95% 的值都位于均值两侧 precision
的范围内。

## 抽样与置信区间

### 17.2 中心极限定理

1、给定一组从同一总体中抽取的足够大的样本，这些样本的均值（样本均值）大致服从正态分布；

2、这个正态分布的均值近似等于总体均值；

3、样本均值的方差近似等于总体方差除以样本量。

## 18.理解实验数据

### 18.1 弹簧的行为

胡克定律：F = -kx

知道某个弹簧的弹簧常数是非常重要的。

通过实验数据可以得到k

## 19.随机试验与假设检验

我们会发现虽然自己手里有很多有价值的数据，但几乎没有分析数据的理论知识。这种情况下，我们经常采取的方法是，使用计算技术建立一个能够拟合数据的模型，然后逐渐形成相关的理论。 

在两组数据中，一组的均值低于另外一组是很平常的事情，而且这种均值之间的差异很可能出于偶然。 

最常用的检验统计量是t统计量。

P-值的含义很容易被误解，它经常被认为是原假设为真的概率，但实际上不是。 

## 20.条件概率与贝叶斯统计

频率学派VS贝叶斯学派

构成贝叶斯推理基础的核心思想就是条件概率 

贝叶斯定理可以迭代使用：观测到一些新证据之后，可以将原来的后验概率作为先验概率，并根据新的证据计算出新的后验概率。 

## 24.分类方法

### 分类器评价

准确率

召回率（灵敏度，真阳性率）

精度

特异度（真阴性率）

假阳性率（1-特异度）

baseline: 当我们检查各种复杂分类算法的性能时，都要和这个基准进行比对。

ROC曲线：绘制多个决策阈值的真阳性率（灵敏度，召回率）和假阳性率（1-特异度）之间的关系。

AUC：ROC曲线下面积，这个面积实际上是个概率，对于一个随机选择的阳性样本，一个好的模型将其标注为阳性的概率应该高于将一个随机选择的阴性样本标注为阳性的概率。