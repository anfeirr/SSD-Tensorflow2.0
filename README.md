# SSD-Tensorflow2.0
## tensorflow 2.0 ssd
vgg 分类网络
Conv2D 层
MaxPool 层
在 conv5 之后接上flatten 后接上三个全连接层，
前面连个激活函数为relu,最后一层的激活函数为softmax
```
python nets/vgg.py
```
```
model.fit_generator(generator=train_batch_generator,
                                  steps_per_epoch=120,
                                  epochs=10,
                                  verbose=1,
                                  validation_data=test_batch_generator,
                                  validation_steps=1)
```
在测试集上的结果能达到90%
现在讲全连接层改成卷积层，试下效果

```
self.fc6 = Conv2D(4096,(7,7),padding="valid",activation='relu',name='conv6')
self.dropout6 = Dropout(0.5)

self.fc7 = Conv2D(4096,(1,1),activation='relu',name='conv7')
self.dropout7 = Dropout(0.5)

self.fc8 = Conv2D(classes,(1,1),padding='same',activation=None,name='conv8')
```
ssd 对 vgg 做了一点变化

在fc6层和fc7层与以往的VGG不同的是，使用卷积层来代替以前的全连接层
卷积层使用的是局部信息，全连接层使用的是全局信息
但是在最后fc6 和 fc7 特征尺寸变大，

可以让卷积网络在一张更大的输入图片上滑动，得到每个区域的输出（这样就突破了输入尺寸的限制）。