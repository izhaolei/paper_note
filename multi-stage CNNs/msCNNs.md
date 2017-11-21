#### Temporal Action Localization in Untrimmed Videos via Multi-stage CNNs

##### abstract

​	本文主要介绍了一个多级的**3D-CNN**结构网络在action localization中的应用，作者提出的一个包括了候选网络、分类网络、定位网络在内的三层网络结构来提升定位的性能。*(该论文是2016cvpr,相对于2017年的文章在准确度上没有那么高，但是文中的proposal数据处理，以及数据的后处理值得借鉴。)*

##### 主要结构

###### 1. 数据生成

​	文中的由于采用**C3D**结构，需求数据的传入为16帧，作者首先将数据按照75%的重合度进行多尺度的滑窗**(滑窗尺度为[16,,32,64,128,256,512])**，对于得到的不同长度的segment进行均匀的下采样到16帧，作为后续网络的输入。*(SCN网络中的数据生成方式可能更好)*

###### 2.网络架构

![](./figure1.png)

​	文中网络主要分为三级，第一级对生成的segment进行初步筛选，去除大量干扰的背景segment; 第二级网络为分类网络，将第一级网络筛选后的数据传入网络，获得该segment所属的类别; 第三极网络为定位网络，相对于分类网络，定位网络在loss function上加入了overlap的惩罚项(如下)。![](./formula1.png)

​	加入overlap惩罚项后，相比如单纯的softmax的crossentropy损失函数，能将该segment与groundtruth之间的重合度纳入考量范围，有效的避免出现高score与overlap间不匹配对后续NMS处理造成的干扰问题。加入overlap惩罚项后loss的曲线如下：![](./figure2.png)