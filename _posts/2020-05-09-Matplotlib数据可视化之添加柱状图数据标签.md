---
layout: post
title:  "Matplotlib: plt.text()给图形添加数据标签"
date:   2020-05-08 14:46:18 +0800
categories: [2020年]
tags: [Python]
comments: 1
---

#### Matplotlib: plt.text()给图形添加数据标签  
##### 数据可视化呈现的最基础图形就是：柱状图、水平条形图、折线图等  
> 在Python的matplotlib库中分别可用bar、barh、plot函数来构建它们，再使用xticks和yticks（设置坐标轴刻度）、xlabel和ylabel（设置坐标轴标签）、title（标题）、legend（图例）、xlim和ylim（设置坐标轴数据范围）、grid（设置网格线）等来装饰图形  

&emsp;&emsp;下面是一个柱状图添加数据标签的例子：  
``` python
import matplotlib.pyplot as plt
import numpy as np
#创建带数字标签的直方图
numbers = list(range(1,11))
#np.array()将列表转换为存储单一数据类型的多维数组
x = np.array(numbers)
y = np.array([a**2 for a in numbers])
# plt.bar()绘制柱形图，x为x轴的位置序列，y为柱子的高度
plt.bar(x,y,width=0.5,align='center',color='c')
# 设置图像的标题
plt.title('Square Numbers',fontsize=24)
# 设置x, y轴的标签
plt.xlabel('Value',fontsize=14)
plt.ylabel('Square of Value',fontsize=14)
# 参数labelsize用于设置刻度线标签的字体大小
plt.tick_params(axis='both',labelsize=14)
# 设置x, y轴的范围（x轴起始于0，终止于11，y轴起始于0，终止于110）
plt.axis([0,11,0,110])
# 通过zip()函数和for循环添加数据标签
for a,b in zip(x,y):
    plt.text(a,b+0.05,'%.0f'%b,ha = 'center',va = 'bottom',fontsize=7)
# 保存绘制的图像
plt.savefig('images\squares.png')
plt.show()
```
&emsp;&emsp;首先，前边设置的x、y值其实就代表了不同柱子在图形中的位置（坐标），通过for循环找到每一个x、y值的相应坐标——a、b，再使用plt.text在对应位置添文字说明来生成相应的数字标签，而for循环也保证了每一个柱子都有标签。其中，a, b+0.05表示在每一柱子对应x值、y值上方0.05处标注文字说明，'%.0f' % b,代表标注的文字，即每个柱子对应的y值，其中0表示不显示小数后面的数值，1就表示显示小数后面一位，以此类推； ha='center', va= 'bottom'代表horizontalalignment（水平对齐）、verticalalignment（垂直对齐）的方式，fontsize则是文字大小。  
&emsp;&emsp;条形图、折线图也是如此设置，饼图则在pie命令中有数据标签的对应参数。对于累积柱状图、双轴柱状图则需要用两个for循环，同时通过a与b的不同加减来设置数据标签位置。
&emsp;&emsp;绘制的图像如下：  
![01](./blog_img/2020-05-08-matplotlib添加数据标签/01.png)
&emsp;&emsp;在我的项目应用中，需要把算法识别到的不同动作次数用柱状图可视化表示出来。参考上面的例子:  
```python
# 绘制结果柱状图
plt.figure(figsize=(8, 6), dpi=80)
plt.subplot(1,1,1)
# 动作类别数
N=4 
values = (action1, action2, action3, action4)
index = np.arange(N)
plt.bar(index, values, width=0.35)
# 设置x, y标签和标题
plt.xlabel('Action')
plt.ylabel('Times')
plt.title('Statistics of action times')
# 设置x轴的标签
plt.xticks(index,('sit', 'stand', 'wave_one_hand', 'wave_two_hands'))
# 在柱子上添加数据标签
for a,b in zip(index, values):
    plt.text(a, b+0.05,'%.0f'% b, ha = 'center', va = 'bottom', fontsize = 10)
plt.savefig('./result.png')
plt.show()
```
&emsp;&emsp;最终绘制的图像为：  
![02](./blog_img/2020-05-08-matplotlib添加数据标签/02.png)  



参考：  
> [柱状图添加数据标签](https://www.cnblogs.com/charliedaifu/p/9964095.html)  
> [Matplotlib中的bar函数](https://www.cnblogs.com/always-fight/p/9707727.html)  
> [matplotlib.pyplot.axis()结构及用法](https://blog.csdn.net/The_Time_Runner/article/details/89888206)  
> [plt.tick_params参数](https://blog.csdn.net/qq_42999481/article/details/82527246)  
> [Python zip()函数](https://www.runoob.com/python/python-func-zip.html)