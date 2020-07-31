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



## 数据集

| Dataset        | Annotation     | Orientation   | Language            | Task                  | End-to-end |
| -------------- | -------------- | ------------- | ------------------- | --------------------- | ---------- |
| ICDAR 2003     | Character/word | Horizontal    | English             | Detection/recognition | Yes        |
| ICDAR 2011     | Word           | Horizontal    | English             | Detection/recognition | Yes        |
| ICDAR 2013     | Character/word | Horizontal    | English             | Detection/recognition | Yes        |
| ICDAR 2015     | Word           | Multi oriente | English             | Detection/recognition | Yes        |
| Incidental     |                |               |                     |                       |            |
| **ICDAR 2017** | Word           | Multi oriente | **Multi lingual**   | Detection/recognition | Yes        |
| MLT            |                |               |                     |                       |            |
| **MSRA-TD500** | Text line      | Multi oriente | **English/Chinese** | Detection             | No         |
| COCO-Text      | Word           | Horizontal    | English             | Detection/recognition | Yes        |
| SVT            | Word           | Horizontal    | English             | Detection/recognition | Yes        |
| **RCTW-17**    | Text line      | Multi oriente | **Chinese**         | Detection             | Yes        |
| IT 5k          | Character/word | Horizontal    | English             | Recognition           | No         |
| SynthText      | Character/word | Horizontal    | English             | Detection/recognition | No         |
| Synth90 k      | Word           | Horizontal    | English             | Recognition           | No         |



其中IDCAR 2017，MSRA-TD500，RCTW-17 包含中文数据集。

## 模型

目前文本检测领域的深度学习方法主要包括：**基于候选框的文本检测(Proposal-based)、基于分割的文本检测(Segmentation-based)、基于两者方法混合的文本检测(Hybrid-based)、其它方法的文本检测**．对于基于候选框的文本检测，其基本思路是先利用若干个default boxes(也称anchor)产生大量的候选文本框，再经过NMS得到最终的检测结果．对于基于分割的文本检测，其基本思路是通过分割网络结构进行像素级别的语义分割，再基于分割的结果构建文本行．

![](https://pic1.zhimg.com/v2-f67afae23c8a07dd7fba350543642c9a_1440w.jpg?source=172ae18b)

整理在ICADR、MSRA TD500、RCTW-17等数据集上的最佳模型。

| Dataset    | Model                                             | F-score               | Precision    | Recall     |
| ---------- | ------------------------------------------------- | --------------------- | ------------ | ---------- |
| ICDAR2017  | [PSENet](https://arxiv.org/abs/1903.12473)        | 72.45(+0.05)          | 77.01(+3.00) | 68.4(-2.2) |
| ICDAR2015  | [IncepText](https://arxiv.org/pdf/1805.01167.pdf) | 90.5                  | 93.8         | 87.3       |
| ICDAR2013  | [FOTS](https://arxiv.org/abs/1801.01671)          | 92.8(+2.80)           |              |            |
| MSRA TD500 | [IncepText](https://arxiv.org/pdf/1805.01167.pdf) | 83(+1.0)              | 87.5         | 79         |
| RCTW-17    | [RRD](https://arxiv.org/abs/1803.05265)           | 67(+1.0 vs IncepText) | 77.5         | 59.1       |
| COCO-Text  | [RRD](https://arxiv.org/abs/1803.05265)           | 67                    |              |            |

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





