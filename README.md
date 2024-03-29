# NLP-CHUNKING
## 项目实验环境

- [ ] Python 2.x
- [ ] CentOS 7.0
- [ ] CRF++-0.58

## 采用条件随机场(CRF++)工具，对训练样本进行训练，得到训练模型
- [x] **模型训练过程**
```bash
% crf_learn template_file train_file model_file
```
这个训练过程的时间、迭代次数等信息会输出到控制台上（感觉上是crf_learn程序的输出信息到标准输出流上了），如果想保存这些信息，我们可以将这些标准输出流到文件上，命令格式如下：

```bash
% crf_learn template_file train_file model_file >> train_info_file
```

有四个主要的参数可以调整：
```bash
-a CRF-L2 or CRF-L1
```
规范化算法选择。默认是CRF-L2。一般来说L2算法效果要比L1算法稍微好一点，虽然L1算法中非零特征的数值要比L2中大幅度的小。
```bash
-c float
```
这个参数设置CRF的hyper-parameter。`c的数值越大，CRF拟合训练数据的程度越高`。**这个参数可以调整过度拟合和不拟合之间的平衡度。这个参数可以通过交叉验证等方法寻找较优的参数**。
```bash
-f NUM
```
这个参数设置特征的cut-off threshold。CRF++使用训练数据中至少NUM次出现的特征。默认值为1。当使用CRF++到大规模数据时，只出现一次的特征可能会有几百万，这个选项就会在这样的情况下起到作用。
```bash
-p NUM
```
如果电脑有多个CPU，那么那么可以通过多线程提升训练速度。NUM是线程数量。

带两个参数的命令行例子：
```bash
% crf_learn -f  3 -c 1.5 template_file train_file model_file
```
## 项目指南
- [x] 给定训练数据样例
在该任务中我们会给定训练文件，训练文件中的样例如下所示：
```bash
[国务院/nt  侨办/j]BNT  发表/v  [新年/t  贺词/n]BNP  本报/r  北京/ns  [12月/t  30日/t]BTP  讯/ng  [新华社/nt  记者/n]BNP  [胡/nr  晓梦/nr]BNP  
```
- [x] 组块类型定义如下表所示

**组块(Chunk)被定义为具有稳定的内部结构和独立语义角色的非重叠和非递归短语。一个组块由两个或多个词语组成，其中一个词为核心词。**

| Category  | Description                 | Example                 | 	
| :---:     |  :------:                   | :----:		              |
| BNP  		  | 	Base noun phrase	        |  [市场/n 经济/n]NP      |	
| BAP     	|   Base adjective phrase 	  |  [公正/a合理/a]BAP      |			
| BVP      	|   Base verb phrase 	        |  [顺利/a启动/v]BVP 	    |		
| BDP  	  	| 	Base adverb phrase        |  [已/d 不再/d]BDP       |			
| BQP    		|   Base quantifier phrase 	  |  [数千/m名/q]BQP 士兵/n |		
| BTP      	|   Base time phrase          |  [早上/t ８时/t]BTP     |		
| BFP  	  	|	  Base position phrase      |  [内蒙古/ns东北部/f]BFP |		
| BNT     	|   Name of an organization	  |  [烟台/ns 大学/n]BNT    |			
| BNS      	|   Name of a place 	        |  [江苏省/ns铜山县/ns]BNS|		
| BNZ  	  	| 	Other proper noun phrase	|  [诺贝尔/nr奖/n]BNZ     |		
| BSV     	|   S-V structure     	      |  [领土/n 完整/a]BSV     |			

- [x] 数据预处理以及后处理
 
如文中的描述所示，这里给出处理后的样本形式，如下：

![NO1](http://o84hyclg0.bkt.clouddn.com/chunkformat.png)


- [x] 模型训练过程以及结果

template模板的编写如下图所示：

![NO2](http://o84hyclg0.bkt.clouddn.com/chunktemp.png)

## **最后这里给出相关数据集(仅供学习勿用作商业)**

下面给出项目中使用的数据集合以及CentOS7.0下的CRF++工具包的百度网盘链接**点击其中一个即可**：

[训练数据集](https://pan.baidu.com/s/19PwxGVgLcfHjTuBlG9fx9A?pwd=12ab)

[测试数据集](http://pan.baidu.com/disk/home#list/path=%2Fchunking)

[CRF++-0.58](https://pan.baidu.com/s/1zcHRMGH_4JbCQ1FH-BBpUQ?pwd=12ab)

## 共同学习

- 邮箱：<honnyzhou@gmail.com>
