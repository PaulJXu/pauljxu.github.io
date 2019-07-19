---
layout: post
title: 关于说话人识别的一些基础知识
subtitle: Speaker recognition
header-style: text
author: jerpson
tags:
    - machine learning
    - speaker recognition
---

# Speaker recognition

说话人识别或称声纹识别，下面是关于这方面技术的一些基础知识整理。

## speaker identification vs. verification

1. speaker identification

* the goal is to match a voice sample from an unknown speaker to one of several of labeled speaker models

* open-set identification: it is possible that the unknown speaker is not in the set of speaker models. if no satisfactory match is found, a no-match decision is provided
* Closed-set: the unknown speaker is one of the known speakers
* performance degrades as the number of comparisons increases.

2. speaker verification

* user makes a claim as to his/her identity, and the goal is to determine the authenticity of the claim

![1553670842422](/img/in-post/1553670842422.png)

![1553670939964](/img/in-post/1553670939964.png)

## text-dependent vs. text-independent

1. text-dependent recognition
   * recognition system knows the text spoken by the person.
   * suited for security applications. to prevent impostors from playing back recorded passwords from authorized speakers, random prompted phrases can be used
2. text-independent recognition
   * recognition system does not know text spoken by person, which could be user-selected phrases or conversational speech.
   * suited for identification of uncooperative speakers.
   * more flexible system but also more difficult problem.

## measurement of speaker characteristics

1. low-level features
   * short-time spectra, generally MFCCs
   * dynamic information. derivatives and second derivatives of the above features are also useful(both for speech and for speaker recognition)
2. high-level features

## Construction of speaker models

1. Non-parametric models
2. Parametric models

## Applications

1. Transaction authentication
2. access control

## references

![](/img/in-post/1553674772146.png)

![1553674794244](/img/in-post/1553674794244.png)

![1553674901482](/img/in-post/1553674901482.png)

## MFCC(Mel Frequency Cepstral Coefficents)

MFCCs在人工特征方面可谓是鹤立鸡群，一枝独秀，从未被超越啊（至于说Deep Learning的特征学习那是后话了）

MFCC(Mel-frequency cepstral coefficients):梅尔频率倒谱系数。梅尔频率是基于人耳听觉特性提出来的， 它与Hz频率成非线性对应关系。梅尔频率倒谱系数(MFCC)则是利用它们之间的这种关系，计算得到的Hz频谱特征。主要用于语音数据特征提取和降低运算维度。例如：对于一帧有512维(采样点)数据，经过MFCC后可以提取出最重要的40维(一般而言)数据同时也达到了将维的目的。

### MFCC提取过程

MFCC一般会经过这么几个步骤：预加重，分帧，加窗，快速傅里叶变换(FFT)，梅尔滤波器组，离散余弦变换(DCT)。其中最重要的就是FFT和梅尔滤波器组，这两个进行了主要的降维操作。

1. 先对语音进行预加重、分帧和加窗：
   * 分帧：为了方便对语音分析，可以将语音分成一个个小段，称之为：帧。先将N个采样点集合成一个观测单位，称为帧。通常情况下N的值为256或512，涵盖的时间约为20~30ms左右。为了避免相邻两帧的变化过大，因此会让两相邻帧之间有一段重叠区域，此重叠区域包含了M个取样点，通常M的值约为N的1/2或1/3。通常语音识别所采用语音信号的采样频率为8KHz(每秒采集8000个点)或16KHz，以8KHz来说，若帧长度为256个采样点，则对应的时间长度是256/8000×1000=32ms。
   * 加窗：语音在长范围内是不停变动的，没有固定的特性无法做处理，所以将每一帧代入窗函数，窗外的值设定为0，其目的是消除各个帧两端可能会造成的信号不连续性。常用的窗函数有方窗、汉明窗和汉宁窗等，根据窗函数的频域特性，常采用汉明窗。

2. 对每一个短时分析窗，通过FFT得到对应的频谱；**（获得分布在时间轴上不同时间窗内的频谱）**

3. 将上面的频谱通过Mel滤波器组得到Mel频谱；**（通过Mel频谱，将线形的自然频谱转换为体现人类听觉特性的Mel频谱）**

4. 在Mel频谱上面进行倒谱分析（取对数，做逆变换，实际逆变换一般是通过DCT离散余弦变换来实现，取DCT后的第2个到第13个系数作为MFCC系数），获得Mel频率倒谱系数MFCC，这个MFCC就是这帧语音的特征；**（倒谱分析，获得MFCC作为语音特征）**

   ![](/img/in-post/20130623210522390.jpg)

![](/img/in-post/20130623210540968.jpg)

![](/img/in-post/1553742373644.png)

### 声谱图(spectrogram)

![](/img/in-post/20130623205844093.jpg)

这段语音被分为很多帧，每帧语音都对应于一个频谱（通过短时FFT计算）

![](/img/in-post/20130623205928484.jpg)

我们先将其中一帧语音的频谱通过坐标表示出来，如上图左。现在我们将左边的频谱旋转90度。得到中间的图。然后把这些幅度映射到一个灰度级表示（也可以理解为将连续的幅度量化为256个量化值？），0表示黑，255表示白色。幅度值越大，相应的区域越黑。这样就得到了最右边的图。那为什么要这样呢？为的是增加时间这个维度，这样就可以显示一段语音而不是一帧语音的频谱，而且可以直观的看到静态和动态的信息。

![](/img/in-post/20130623210019187.jpg)

![](/img/in-post/20130623210044156.jpg)

## references

https://www.cnblogs.com/BaroC/p/4283380.html

<http://practicalcryptography.com/miscellaneous/machine-learning/guide-mel-frequency-cepstral-coefficients-mfccs/>

<http://www.speech.cs.cmu.edu/15-492/slides/03_mfcc.pdf>

Speaker recognition lecture.pdf

