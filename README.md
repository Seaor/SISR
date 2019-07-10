# SISR
Single Image Super Resolution

2019/7/1 图像退化

下周主要关注树的局部扰动
处理低清图，24来自训练集1，其他来自训练集2，主要是针对树和水面进行数据增广，水面主要参照水面问题比较明显的测试集中的0026，树参照0017等图片，共8张，基本是运动模糊，局部横向运动模糊，低通滤波平滑，白色遮罩和光照调整等

2019/7/3 训练代码

1. 将服务器文件夹映射到自己本地，方便读写

2. 训练代码 python codes/train.py -opt codes/options/train/train_ESRGAN.json
主要说了一下codes/options/train 中的训练配置文件 train_ESRGAN.json的各个参数什么意思
    
测试同理 python codes/test.py -opt codes/options/test/test_ESRGAN.json
主要说了一下codes/options/test 中的训练配置文件 test_ESRGAN.json的各个参数什么意思

好像都不用改什么，GPU我用4，5，网络是2x的

3.然后说了一下做数据的过程

首先在training_data 中放入处理过后的整图，使用process_dataset.py 随机裁剪LR-HR数据对,裁成小patch
（这个地方如果想取对应的图片部分，比如树木或者建筑水面之类的，先自己获取该区域的左上角右下角点的坐标，然后给图片标号放在一个文件夹下，把图片的编号、区域的位置信息用#分割放在一个txt下面，然后再用process_dataset.py处理成小patch）

第二步制作在训练配置文件train_ESRGAN.json中使用的lmdb文件，在code/scripts/create_lmdb.py文件中，填好输入和输出路径对hr和lr分别做一遍，得到lmdb

4.将随机裁剪成小patch的结果放到training_data/64_128_data_0629下面的文件夹中和原先的数据一起做fine-tuning，裁2000差不多就够了。
