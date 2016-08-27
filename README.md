# NLP-CHUNKING
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
给定训练数据样例
在该任务中我们会给定训练文件，训练文件中的样例如下所示：
[国务院/nt  侨办/j]BNT  发表/v  [新年/t  贺词/n]BNP  
本报/r  北京/ns  [12月/t  30日/t]BTP  讯/ng  [新华社/nt  记者/n]BNP  [胡/nr  晓梦/nr]BNP  、/w  [本报/r  记者/n]BNP  [吴/nr  亚明/nr]BNP  报道/v  ：/w  新年/t  [将/d  至/v]BVP  ，/w  [国务院/nt  侨务/n  办公室/n]BNT  主任/n  [郭/nr  东坡/nr]BNP  今天/t  通过/p  [新闻/n  媒介/n]BNP  ，/w  向/p  [海外/s  同胞/n]BNP  和/c  [国内/s  归侨/n]BNP  、/w  侨眷/n  、/w  [侨务/n  工作者/n]BNP  发表/v  [新年/t  贺词/n]BNP  。/w  
他/r  代表/v  [中华人民共和国/ns  国务院/nt]BNT  [侨务/n  办公室/n]BNT  ，/w  向/p  广大/b  [海外/s  同胞/n]BNP  和/c  [国内/s  归侨/n]BNP  、/w  侨眷/n  、/w  [侨务/n  工作者/n]BNP  ，/w  致以/v  [亲切/a  的/u  问候/vn]BNP  和/c  [美好/a  的/u  祝愿/vn]BNP  。/w  
[贺词/n  说/v]BSV  ，/w  [即将/d  过去/v]BVP  的/u  １９９７年/t  [是/v  我国/n]BVP  [历史/n  发展/v]BNP  上/f  的/u  重要/a  [一/m  年/q]BTP  。/w  
我国/n  恢复/v  对/p  香港/ns  [行使/v  主权/n]BVP  ，/w  五星红旗/n  在/p  香港/ns  [庄严/ad  升起/v]BVP  ，/w  [饱受/v  屈辱/n]BVP  的/u  香港/ns  [终于/d  又/d]BDP  [回到/v  了/u]BVP  [祖国/n  的/u  怀抱/n]BNP  ；/w  
９月/t  ，/w  [中国/ns  共产党/n]BNT  [第十五/m  次/q]BQP  [全国/n  代表大会/n]BNP  [胜利/vd  召开/v]BVP  ，/w  大会/n  对/p  我国/n  [改革/v  开放/v]BVP  和/c  [现代化/vn  建设/vn]BNP  的/u  [跨/v  世纪/n]BVP  发展/vn  [作出/v  了/u]BVP  [战略/n  部署/vn]BNP  ，/w  [描绘/v  出/v]BVP  [宏伟/a  蓝图/n]BNP  。/w  
