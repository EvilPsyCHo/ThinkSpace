# 文本检测概述

当我们谈起计算机视觉时，首先想到的就是图像分类，没错，图像分类是计算机视觉最基本的任务之一，但是在图像分类的基础上，还有更复杂和有意思的任务，如目标检测，物体定位，图像分割等，见下图所示。其中目标检测是一件比较实际的且具有挑战性的计算机视觉任务，其可以看成图像分类与定位的结合，给定一张图片，目标检测系统要能够识别出图片的目标并给出其位置。

![](https://ss2.baidu.com/6ON1bjeh1BF3odCf/it/u=2507324046,3395120537&fm=15&gp=0.jpg)

## 目标检测评估方法

目标检测采用**AP(Average Precision)**即平均准确率作为衡量模型表现的方法。

- 计算IoU(Intersection over union)

  真实标签与预测标签面积的交际除以面积并集

  ![](https://www.pyimagesearch.com/wp-content/uploads/2016/09/iou_result_02.jpg)

  ![](https://user-images.githubusercontent.com/15831541/37725175-45b9e1a6-2d2a-11e8-8c15-2fb4d716ca9a.png)

- 得到PR曲线(Precision Recall Curve)

  设置某个阈值，预测结果（Positive）IoU高于阈值的样本正确（TP），低于某个阈值的样本错误（FP），没有检测到的标签为（FN)，TN样本为0，因此我们通过改变阈值，得到模型预测结果的PR曲线。

- PR曲线平滑
  
  PR曲线每个点，用曲线右侧的最大P值填充，平滑曲线。
  
  ![](https://user-images.githubusercontent.com/15831541/43008995-64dd53ce-8c34-11e8-8a2c-4567b1311910.png)
  
- 计算RP曲线AUC做为该类别识别平均精度AP

- 计算多类别均值mAP

## 数据集

*Det - Detection, Rec - Recogonition, C - Character, W - Word, T - TextLine*

| DataSet                                                      | Language        | Task    | Annotation |
| ------------------------------------------------------------ | --------------- | ------- | ---------- |
| [CTW data](https://ctwdataset.github.io/)                    | Chinise         | Det/Rec | C/W/T      |
| ICDAR 2003                                                   | English         | Det/Rec | C/W        |
| ICDAR 2011                                                   | English         | Det/Rec | C/W        |
| ICDAR 2013                                                   | English         | Det/Rec | C/W        |
| ICDAR 2015                                                   | English         | Det/Rec | W          |
| ICDAR 2017                                                   | Multi           | Det/Rec | W          |
| ICDAR 2019                                                   | Multi           | Det/Rec | C/T        |
| **MSRA-TD500**                                               | English/Chinese | Det     | T          |
| RCTW-17                                                      | Chiniese        | Det     | T          |
| [WTMI](https://tianchi.aliyun.com/competition/entrance/231685/information) | Chiniese        | Det     | C/T        |
| SyntheticChineseStringDataset                                | Chiniese        |         |            |
| ....                                                         |                 |         |            |

除了真实样本数据集外，还有开源的数据集生成脚本：

- [SynthText_Chinese_version](https://github.com/JarveeLee/SynthText_Chinese_version)
- ...

## 模型

目前文本检测领域的深度学习方法主要包括：**基于候选框的文本检测(Proposal-based)、基于分割的文本检测(Segmentation-based)、基于两者方法混合的文本检测(Hybrid-based)、其它方法的文本检测**．对于基于候选框的文本检测，其基本思路是先利用若干个default boxes(也称anchor)产生大量的候选文本框，再经过NMS得到最终的检测结果．对于基于分割的文本检测，其基本思路是通过分割网络结构进行像素级别的语义分割，再基于分割的结果构建文本行．

![](https://pic1.zhimg.com/v2-f67afae23c8a07dd7fba350543642c9a_1440w.jpg?source=172ae18b)

整理在ICADR、MSRA TD500、RCTW-17等数据集上的最佳模型（截止2019），其中括号内为基线模型CTPN表现。

| Dataset    | Model                                             | F-score               | Precision | Recall    |
| ---------- | ------------------------------------------------- | --------------------- | --------- | --------- |
| ICDAR2017  | [PSENet](https://arxiv.org/abs/1903.12473)        | 72.45                 | 77.01     | 68.4      |
| ICDAR2015  | [IncepText](https://arxiv.org/pdf/1805.01167.pdf) | 90.5 (61)             | 93.8 (74) | 87.3 (52) |
| ICDAR2013  | [FOTS](https://arxiv.org/abs/1801.01671)          | 92.8 (88)             |           |           |
| MSRA TD500 | [IncepText](https://arxiv.org/pdf/1805.01167.pdf) | 83(+1.0)              | 87.5      | 79        |
| RCTW-17    | [RRD](https://arxiv.org/abs/1803.05265)           | 67(+1.0 vs IncepText) | 77.5      | 59.1      |
| COCO-Text  | [RRD](https://arxiv.org/abs/1803.05265)           | 67                    |           |           |

- CTPN

  采用VGG作为特征提取器，conv5层输出的特征图 N x C x W x H 采用 3 x 3 卷积处理后得到 N x 9C x W x H特征向量，转换维度到 NH x W x 9C ，将W（也就是从左到右的视线顺序作为时间维度）作为时间维度输入到双向LSTM中，最后经过RPN层，回归得到文本框最标回归值及分类；模型结构导致只能识别水平文本。

- EAST（paddleOCR）

  Efficient and Accuracy Scene Text，将特征提取器（VGG/PVANet/ResNet）不同空间尺度特征进行合并融合，输出原始照片像素属于文本的得分图、像素到文本矩形4条边的距离及矩形角度、矩形4个定点的偏移量。优势是端到端、支持任意4边形文本框。

- DB（paddleOCR）

  Differentiable Binarization 模块，可微分二值化模块。为了解决基于像素点预测的图像分割后处理过程复杂，需要通过固定阈值判断+启发式搜索确定切割区域的问题，阈值不同对性能影响较大。DB提供对每一个像素点进行自适应二值化，二值化阈值由网络学习得到，彻底将二值化这一步骤加入到网络里一起训练，这样最终的输出图对于阈值就会非常鲁棒。

  像素二值化公式：
  $$
  B_{ij} = 1 / (1+e^{-k(P_{ij}-T_{ij})})
  $$
  其中$P_{ij}$表示网络输出像素点属于文章的概率，$T_{ij}$表示像素阈值。

  论文显示DB模块在连续水平、多方向、多语言等数据集上取得最佳性能，推理速度较快，采用轻量级骨干网络依然能够得到较好的精度。
  
   https://github.com/MhLiao/DB 
  
   https://github.com/WenmuZhou/DBNet.pytorch 
  
- SSD: Single Shot MultiBox Detector

## 开源项目

- [DBNet PyTorch](https://github.com/MhLiao/DB)

  PyTorch版本可微分二值化网络，精度复线论文水平，提供完善的训练、推理接口。

- [PaddleOCR PaddlePaddle]()

  实现了EAST、DB文本检测算法，Rosetta、CRNN、Star-Net、RARE文本识别算法，全平台支持。

## 参考

- [how-to-calculate-map-for-detection-task-for-the-pascal-voc-challenge]( https://datascience.stackexchange.com/questions/25119/how-to-calculate-map-for-detection-task-for-the-pascal-voc-challenge )
- [map-mean-average-precision-for-object-detection](https://medium.com/@jonathan_hui/map-mean-average-precision-for-object-detection-45c121a31173)
- [Face++ 深度学习时代的文字检测与识别技术](https://zhuanlan.zhihu.com/p/51725259)
- [Review of Scene Text Detection and Recognition, [*Archives of Computational Methods in Engineering*], 2020 ](https://link.springer.com/article/10.1007/s11831-019-09315-1)
- [Total-Text-Dataset 英文开源数据集及评估脚本](https://github.com/cs-chan/Total-Text-Dataset)
- [Curve-Text-Detector 中文开源数据集及评估脚本](https://github.com/Yuliang-Liu/Curve-Text-Detector)
- [OCR开源数据集汇总及转换脚本](https://github.com/WenmuZhou/OCR_DataSet)
- [OCR开源数据集汇总 - CSDN](https://blog.csdn.net/buptlihang/article/details/99578415)
- [OCR 开源数据集汇总 - github](https://github.com/xylcbd/ocr-open-dataset)
- [知乎：场景文字检测—CTPN原理与实现](https://zhuanlan.zhihu.com/p/34757009)
- [知乎：TextBox](https://zhuanlan.zhihu.com/p/43545190)
- [*Real-time Scene Text Detection with Differentiable Binarization*](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1911.08947)
- [知乎：自然场景文本检测识别技术综述]( https://zhuanlan.zhihu.com/p/38655369 )
- [知乎：目标检测最新进展和展望]( https://zhuanlan.zhihu.com/p/46595846 )

 https://blog.csdn.net/buptlihang/article/details/99578415 

 https://github.com/Sierkinhane/CRNN_Chinese_Characters_Rec/tree/master 



