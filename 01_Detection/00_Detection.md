# Detection

---

## survey/overview/review

- 香港中文大学，商汤等联合提出的MMDetection，包括检测模型，实体分割等state-of-art模型框架源码，属业界良心。

  - [2019.06][MMDetection: Open MMLab Detection Toolbox and Benchmark](https://arxiv.org/pdf/1906.07155.pdf)

- 西安电子科技大学关于目标检测的论文综述。
  - [2019.07][A Survey of Deep Learning-based Object Detection](https://arxiv.org/pdf/1907.09408.pdf)

- 新加坡管理大学论文综述。

  - [2019.08][Recent Advances in Deep Learning for Object Detection](https://arxiv.org/pdf/1908.03673.pdf)
  
- 中东科技大学(Middle East Technical University)一篇关于目标检测领域imbalance problem的综述。imbalance problem包括Class imbalance，
   Scale imbalance，Spatial imbalance， objective imbalance。论文对各个方面进行归纳，提出问题和分析解决方案。
  **话说imbalance中，头部问题是？**  
  - [2019.09][Imbalance Problems in Object Detection: A Review](https://arxiv.org/pdf/1909.00169.pdf)
  [github](https://github.com/kemaloksuz/ObjectDetectionImbalance)

- 西弗吉尼亚大学提出一种评估目标检测理论准确率上限：91.6% on VOC (test2007), 78.2% on COCO (val2017), and 58.9% on OpenImages V4 (validation)。
  - [2019][Empirical Upper-bound in Object Detection and More](https://arxiv.org/pdf/1911.12451.pdf)
  
- state of art
  - <https://paperswithcode.com/sota/object-detection-on-coco>

---

## Facial Detector

- 天津大学、武汉大学、腾讯AI实验室提出的人脸检测模型，主要针对移动端设计（backbone MobileNet v2)在高通845上达到140fps的实时性。论文主要提出一个解决类别不均衡问题（侧脸、正脸、抬头、低头、表情、遮挡等各种类型）：增加困难类别和样本的损失函数权重。

  -缺点：但是MobileNet v2的检测框架应该没有这么快，并且论文的预测特征点和3D旋转角时，使用全连接网络，计算量大，应该更耗时才对。论文只给出特征点预测，但是一张图片N个人，如何区分特征点属于何人？论文没有告知如何计算检测框。
  - 论文应该很多细节没有讲述清晰，应该是idea分拆，写成连续剧的节奏。

  - [2019.02][PFLD:A Practical Facial Landmark Detector](https://arxiv.org/pdf/1902.10859.pdf)

---

## object Detection

- ICCV2017论文，微软亚洲研究院代季峰等提出DCN(Deformable Convolutional Network),将固定位置的卷积改造为适应物体形变的可变形卷积。
提出两个模块：deformable convolution 和deformable RoI pooling。所谓的deformable，是在原deformable convolution基础上增加可学习的offset(增加感受野范围)。同理RoI pooling计算上增加偏差实现deformable RoI pooling。论文提出的DCN俨然已经是目标检测领域刷分必备插件。

  - 1.针对任意形变的目标，offset都一样？
  - 2.x(p0+pn+Δpn)由于存在小数，通过双线性插值实现。G(q; p)·x(q)和双线性插值什么关系？
  - [2017.03][Deformable Convolutional Networks](https://arxiv.org/pdf/1703.06211.pdf)
  
- Deformable ConvNet V2（DCNv2),DCNv1升级版。论文任务DCNv1虽然可解决变形目标的检测，但是受irrelevant image content影响。论文提出三个改进措施：1、增加更多的Deformable Convolution，Conv3-Conv5的3x3 convolution都换成了Deformable Conv。2、让Deformable Conv不仅能学习offset，还能学习每个采样点的权重（modulation）3、模拟R-CNN的feature（knowledge distillation）。
  - Feature Mimicking如何实现？
  -[2018.12][Deformable ConvNets v2: More Deformable, Better Results](https://arxiv.org/pdf/1811.11168.pdf)
  
- CVPR2018论文，加利福尼亚大学圣迭戈分校提出。出发点是在目标检测中提高IoU阈值可提高准确率（影响召回率）。既然不能直接提高IoU，可以分阶段提高IoU的阈值。Faster R-CNN是RCNN的two stage级联，RPN有分类和回归，NMS抑制后ROIpool继续计算分类和回归。既然这样为什么不多级联几次？

  - 1.Cascade R-CNN是R-CNN的多层级联，损失函数也是级联，那么检测框从那一个detector输出？是都输出？
  - 2.iterative bounding box和Cascade R-CNN框架相同，只是损失函数不同？
  - [2017.12][Cascade R-CNN: Delving into High Quality Object Detection](https://arxiv.org/pdf/1712.00726.pdf)
  
- Cascade R-CNN更新续篇。从网络结构看是增加Instance segmentation分支，检测(ResNeXt-101)45.8AP->(ResNeXt-152)50.2AP,38.6->42.3AP.
  - [2019.06][Cascade R-CNN: High Quality Object Detection and Instance Segmentation](https://arxiv.org/pdf/1906.09756.pdf)

- 国防科技大学和旷视科技联合提出，典型的RPN+FPN架构，backbone基于SNet,增加Context Enhancement
Module(FPN多尺度分辨率特征融合)和spatial attention module（RPN->1x1卷积实现空间注意力模型），
实验结果相对于MobileNetV2-SSDLite速度和精度均有提高。

  - [2019.03][ThunderNet: Towards Real-time Generic Object Detection](https://arxiv.org/pdf/1903.11752.pdf)

- CVPR2019论文、商汤，浙江大学等联合提出的Libra R-CNN。motivation来自于作者认为的三个不平衡：数据不平衡，特征不平衡，
损失函数不平衡。数据不平衡采用：N总样本根据IoU分成K个子样本,增加困难样本的采样概率。特征不平衡采用：ResNet Identity 和
non-local模块修正语义特征。损失函数不平衡：论文设计Balanced L1 Loss（**待验证和理解**）。

  论文提出的三个不平衡，可以认为是3个trick，可以集成到其他模型，改进检测的精度。

  - [Libra R-CNN: Towards Balanced Learning for Object Detection](https://arxiv.org/pdf/1904.02701.pdf)

- 中国科学院大学等提出Trident Networks，既模型backbone包含的tricks:Multi-branch Block,Weight sharing among branches,Scale-aware Training Scheme(不同尺度目标位于不同分支)，模型最终集万千tricks于一身，基于ResNet-101-Deformable，在COCO test-dev set取得state-of-art，48.4 mAP。

  - [2019][Scale-Aware Trident Networks for Object Detection](https://arxiv.org/pdf/1901.01892v1.pdf)
  
- 微软等提出GCNet(global context network),基于Non-Local Network（NLNet）和Squeeze-Excitation Network (SENet)集成版。论文验证对于Non-Local Network，不同的query points可得到一致的attention maps（为什么Non-Local是不一致的attention map?），因此可以简化NL模块为Context Modeling，结合Transform既为Global context (GC) block。个人认为Global context (GC) block是空间注意力机制的延伸。论文模型在集成X101+DCN++Cascade+GC r4，在COCO test-dev set取得state-of-art，48.4 mAP，再次证明集成的伟大。

  - [GCNet: Non-local Networks Meet Squeeze-Excitation Networks and Beyond](https://arxiv.org/pdf/1904.11492v1.pdf)[2019.04]

- 北京大学等提出的一种改善型backbone，类似于HRNet和Cascade R-CNN（Cascade R-CNN是级联detector,而本文CBNet是级联backbone）。论文最强指标Cascade Mask R-CNN +Triple-ResNeXt152在COCO数据集实现53.3AP，性能上是数据榜首。  
  -[2019.09][CBNet: A Novel Composite Backbone Network Architecture for Object Detection](https://arxiv.org/pdf/1909.03625.pdf)

- 韩国高等理工学院提出Cascade RPN。

  -[2019.09][Cascade RPN: Delving into High-Quality Region Proposal Network with Adaptive Convolution](https://arxiv.org/pdf/1909.06720.pdf)

---

## Tiny

- 自滑铁卢大学提出YOLO Nano 的网络，他们通过人与机器协同设计模型架构大大提升了性能。YOLO Nano 大小只有 4.0MB 左右，比 Tiny YOLOv2 和 Tiny YOLOv3 分别小了 15.1 倍和 8.3 倍，性能却有较为显著的提升。

  -[2019.10][YOLO Nano: a Highly Compact You Only Look Once Convolutional Neural Network for Object Detection](https://arxiv.org/pdf/1910.01271.pdf)

- 腾讯，香港科技大学联合提出，One-stage目标检测框架RefineDetLite，解决在CPU场景下实时性问题。backbone基于Res2Net，anchor refinement module (ARM)提出粗略anchor，object detection module (ODM) 融合
特征金字塔特征修订anchor，生成最终的分类信息和检测框。论文的训练策略包括IOU-guided loss对负样本抑制, classes-aware weighting 解决样本类别不平衡和balanced multitask training多任务训练，最终实现
RefineDetLite++在MSCOCO数据集29.6AP&131ms。
  - 论文提出的模型，虽然一直倡导非GPU而是CPU，实际在Intel i7-6700@3.40GHz测试，貌似和mobile实际运行有差距。 
  - 论文提出的RefineDetLite，包含coarse loss module和refined loss module，仅仅是没有ROI pooling，个人感觉和two-stage的目标检测框架没太多区别。
  - [2019][RefineDetLite: A Lightweight One-stage Object Detection Framework for CPU-only Devices](https://arxiv.org/pdf/1911.08855.pdf)  

---

## Imbalance

- 中国科学技术大学提出，在Focal Loss基础上解决目标检测前景和背景不平衡问题。论文主要提出三个观点：decoupling objectness from classification, biased initialization, threshold movement。
不过从数据对比，性能提升不明显。
  - [2019][Revisiting Foreground-Background Imbalance in Object Detectors](https://arxiv.org/pdf/1909.04868.pdf)

---

## loss

  - [Generalized Intersection over Union: A Metric and A Loss for Bounding Box Regression](https://arxiv.org/pdf/1902.09630.pdf)[2019.02]

---

## one-stage

- 商汤和香港中文大学联合提出，ICLR2019论文，实在没看懂啥意思。

  - [Feature Intertwiner for Object Detection](https://arxiv.org/pdf/1903.11851.pdf)[2019.03]
  
- 阿德莱德大学沈春华项目组提出的目标检测方向新论文FCOS,去除传统目标检测的FPN操作,添加Center-ness分支，直接anchor free预测
(l; t; r; b) 四维向量（节省anchor相关的超参数设定以及计算），依赖于NMS，直接生成目标检测框。论文提出的FCOS希望可以应用于
后续的语义分割，姿态估计等领域。论文的性能在one-stage领域state-of-art，节省大量FPN的计算，但是没有任何关于速度的指标，比较遗憾。

  - [FCOS: Fully Convolutional One-Stage Object Detection](https://arxiv.org/pdf/1904.01355.pdf)[2019.04]

- 清华大学提出HAR-Net，在single-stage 目标检测框架FPN-Retina-Net基础上改进，混合实现attention mechanism，包括spatial attention, channel attention和aligned attention。spatial attention通过堆叠空洞卷积实现增加感受视野，channel attention通过squeeze-excitation (SE) block实现，aligned attention通过deformable convolution实现。论文思路清晰，实现单阶段目标检测模型性能的state-of-art(45.8% mAP COCO)，并超越two stage Cascade RCNN。论文没有实时性的数据。

  - [HAR-Net: Joint Learning of Hybrid Attention for Single-stage Object Detection](https://arxiv.org/pdf/1904.11141.pdf)[2019.04]

- 中国科学院大学,牛津大学和华为联合提出one-stage模型，MS-COCO dataset数据集测试，达到47mAP（逼近two-stage PANet准确率）,超越所有one-stage模型。论文模型backbone
基于Hourglass，提出Center pooling，Corner pooling，Cascade corner pooling（论文的triplet）确定目标的边界。
NVIDIA Tesla P100 GPU运行，CenterNet511-104 340ms/image，比CornerNet511-104 300ms略慢。

  - [CenterNet: Object Detection with Keypoint Triplets](https://arxiv.org/pdf/1904.08189.pdf)[2019.04]

- 普林斯顿大学提出CornerNet改进版的目标检测模型，CornerNet-Saccade比CornerNet提速6倍且性能提升1% AP,CornerNet-Squeeze比YOLOv3更快也更准确。
论文首先把图片Downsizing，经过hourglass网络与Attention maps生成候选区域并裁剪，对每个裁剪区域再经过hourglass生成目标检测框。最后对所有检测框合并。
论文使用缩放代替CNN的降采样，较少运算量，但是缩小图像的分辨率对小目标的检测准确率应该有影响吧。个人感觉比CenterNet实用性更好：要么选择准确率高的two-stage,
要么选择速度和准确率平衡的轻量化网络。感叹目标检测领域的飞速发展。CV行业在分类和检测日新月异。

  - [2019.04][CornerNet-Lite: Efficient Keypoint Based Object Detection](https://arxiv.org/pdf/1904.08900.pdf)
  - [github](https://github.com/princeton-vl/CornerNet-Lite)
  
- 中科院自动化所模式识别实验室提出目标检测领域anchor based and anchor-free本质不同：训练模型时正负样本采样策略，相同正负样本情况下可以达到同样的性能；根据检测目标的静态特性，提出ATSS自适应训练样本选择策略(adaptive training sample selection)。
在ASTT改进下， anchor based and anchor-free性能均有提升，且达到state-of-art 50.7%AP。
  - 目标检测一般划分为anchorbased and anchor-free。anchorbased可划分为one stage 和two stage；anchor-free可细分为keypoint-based和center-based。
  - ATSS过程很容易理解，替代直接使用IOU作为候选框的阈值，首先选择ground-truth中心最近的k个候选框，计算均值和方差作为IOU阈值，去除中心点在检测目标之外的候选框。整个过程中只有一个超参数k，且对模型影响小。
  - 论文没有多网络模型任何改进，仅仅是训练时样本的改进，大道至简，返璞归真。
  - [2019][Bridging the Gap Between Anchor-based and Anchor-free Detection via Adaptive Training Sample Selection](https://arxiv.org/pdf/1912.02424v1.pdf)
  - <https://github.com/sfzhang15/ATSS>

- AAAI2020论文，天津大学，中国人民公安大学等提出。IoU预测目标框是目标检测领域的重要组成。常规IoU loss计算方法,仅用于候选框和gt有交叠区域，无交叠时没有梯度生成。GIoU loss惩罚项，但是gt和候选框存在包关系时直接退化为IoU。论文在两个loss基础上引入惩罚项，提出Distance-IoU(DIoU) Loss和Complete IoU(CIoU) Loss用于目标检测BBox进一步回归，收敛速度快，准确率高，容易集成于NMS。
  - DIoU Loss可以直接最小化两个目标框的距离，在包含框在水平或垂直方向时，回归loss下降更快。
  - Complete IoU(CIoU),包含交叠像素面积,中心点距离，长宽比，可以更好描述检测框的位置关系。
  - NMS阶段使用DIoU作为评价尺度，同时考虑IoU和中心点距离，进一步提高目标检测性能。
  - 论文提出的trick，在PASAC2007和COCO2017均明显涨点。在state-of-art模型比如Cascade R-CNN等，是否有进一步良好表现？
  - [2019][Distance-IoU Loss: Faster and Better Learning for Bounding Box Regression](https://arxiv.org/pdf/1911.08287.pdf)

---

## NMS系列

2017----Soft-NMS----Improving Object Detection With One Line of Code

2018----Softer-NMS- Rethinking Bounding Box Regression for Accurate Object Detection

2018----IoUNet----Acquisition of Localization Confidence for Accurate Object Detection

2018----CVPR----Improving Object Localization with Fitness NMS and Bounded IoU Loss

2018----NIPS----Sequential Context Encoding for Duplicate Removal

M2Det: A Single-Shot Object Detector based on Multi-Level Feature Pyramid Network [PDF](https://arxiv.org/pdf/1811.04533.pdf) [Github](https://github.com/qijiezhao/M2Det)

Grid R-CNN [PDF](https://arxiv.org/pdf/1811.12030.pdf)

---

## framwork

[simpledet](https://github.com/tusimple/simpledet)
mmdetection
maskrcnn-benchmark

---

# tricks

1.backbone与特征提取

目标检测的backbone一般基于ImageNet预训练的图像分类网络。图像分类问题只关注分类和感受视野，不用关注物体定位，
但是目标检测领域同时很关注空间信息。如果下采样过多，会导致最后的feature map很小，小目标很容易漏掉。很多基础架构网络，
比如ResNet、Xception、DenseNet、FPN、DetNet、R-CNN，PANet、等神经网络提取图像的上下文信息，不断在特征提取方向优化。

2.基于Anchor生成的算法

比如Sliding window、Region Proposal Network (RPN) 、CornerNet、meta-anchor等。

3.IoU计算

UnitBox，IoU-Net，GIoU

4.损失函数

包括L1和L2，Focal loss，增加困难类别和样本的损失函数权重

5.优化NMS

包括Soft-NMS,Softer-NMS,以及Relation Netwrok，ConvNMS，NMS Network，Yes-Net等。

6.通用tricks

类别不平衡（增加样本）,数据增强

# 待记录

Adaptive NMS: Refining Pedestrian Detection in a Crowd.[pdf](https://arxiv.org/pdf/1904.03629.pdf)


CVPR2019
[All You Need is a Few Shifts: Designing Efficient Convolutional Neural Networks for Image Classification](https://arxiv.org/pdf/1903.05285.pdf)

