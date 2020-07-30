# 文本检测与识别

当我们谈起计算机视觉时，首先想到的就是图像分类，没错，图像分类是计算机视觉最基本的任务之一，但是在图像分类的基础上，还有更复杂和有意思的任务，如目标检测，物体定位，图像分割等，见下图所示。其中目标检测是一件比较实际的且具有挑战性的计算机视觉任务，其可以看成图像分类与定位的结合，给定一张图片，目标检测系统要能够识别出图片的目标并给出其位置。

![](https://ss2.baidu.com/6ON1bjeh1BF3odCf/it/u=2507324046,3395120537&fm=15&gp=0.jpg)

## 目标检测评估

目标检测采用**AP(Average Precision)**即平均准确率作为衡量模型表现的方法。

- 计算IoU(Intersection over union)

  真实标签与预测标签面积的交际除以面积并集

  ![](https://www.pyimagesearch.com/wp-content/uploads/2016/09/iou_result_02.jpg)

- 得到PR曲线(Precision Recall Curve)

  设置某个阈值，预测结果（Positive）IoU高于阈值的样本正确（TP），低于某个阈值的样本错误（FP），没有检测到的标签为（FN)，TN样本为0，因此我们通过改变阈值，得到模型预测结果的PR曲线。

- PR曲线平滑
  
  PR曲线每个点，用曲线右侧的最大P值填充，平滑曲线。



## 参考

[how-to-calculate-map-for-detection-task-for-the-pascal-voc-challenge]( https://datascience.stackexchange.com/questions/25119/how-to-calculate-map-for-detection-task-for-the-pascal-voc-challenge )

[map-mean-average-precision-for-object-detection](https://medium.com/@jonathan_hui/map-mean-average-precision-for-object-detection-45c121a31173)





