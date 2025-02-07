# 基于回归的二手车价格交易预测系统和基于神经协同过滤NCF的推荐系统

# 1.数据来源  
二手车交易价格预测  	
https://tianchi.aliyun.com/competition/entrance/231784/information
数据含义:  

SaleID	交易ID，唯一编码  
name	汽车交易名称，已脱敏  
regDate	汽车注册日期，例如20160101，2016年01月01日  
model	车型编码，已脱敏  
brand	汽车品牌，已脱敏  
bodyType	车身类型：豪华轿车：0，微型车：1，厢型车：2，大巴车：3，敞篷车：4，双门汽车：5，商务车：6，搅拌车：7  
fuelType	燃油类型：汽油：0，柴油：1，液化石油气：2，天然气：3，混合动力：4，其他：5，电动：6  
gearbox	变速箱：手动：0，自动：1  
power	发动机功率：范围 [ 0, 600 ]  
kilometer	汽车已行驶公里，单位万km  
notRepairedDamage	汽车有尚未修复的损坏：是：0，否：1  
regionCode	地区编码，已脱敏  
seller	销售方：个体：0，非个体：1  
offerType	报价类型：提供：0，请求：1  
creatDate	汽车上线时间，即开始售卖时间  
price	二手车交易价格（预测目标）  
v系列特征	匿名特征，包含v0-14在内15个匿名特征  

# 2.数据分析   
1.数据分析.ipynb  
1.通过分析缺失值，['model', 'bodyType', 'fuelType', 'gearbox']列的缺失值为 [1, 4506, 5981, 8680]，因此删除'bodyType', 'fuelType', 'gearbox'列   
2.通过数据类型分析，"notRepairedDamage"列中存在24324个"-"值，考虑用nan进行替换   
3.观测到offerType和seller类别特征严重倾斜，一般不会对预测有什么帮助，故这边先删掉   
4.预测价格不是服从正态分布，因此进行对数变换  
	
# 3.数据预处理 
	dataset.py

# 4.使用xgboost二手车价格预测模型及调参，预测的效果如下图所示：

	regress.py  
![image](https://user-images.githubusercontent.com/82042336/198589544-45fb67cc-309f-4b88-a697-d802403b2f55.png)

# 5.主试验 
	train_experiment.py

  1）由于预测价格不是服从正态分布，因此进行对数变换，变换数据前后进行验证  
  2）采用主成分分析建模和正常数据建模  
  
     最终得出的结果是二手车的预测价格需要进行变换，不使用主成分分析
  
# 6.测试模块 
	test.py
	
# 基于神经协同过滤NCF的推荐系统
	待更新，代码在recommed目录下
	python main.py --model_name NCF --emb_size 64 --layers '[64]' --lr 5e-4 --l2 1e-7 --dropout 0.2 --dataset 'Browsing_History' --epoch 10
.   
│   Neural Collaborative Filtering神经协同过滤.pdf  
│   Self-Attentive Sequential Recommendation.pdf  
│
├─data  
│    ├─Browsing_History  
│    │      generationdata.py  
│    │      pre_data.py  
│    │      结果可视化.png  
│    │  
│    └─RealRecord   
│            data.csv  
│            pre_data.py  
│            结果可视化.png  
│ 
├─log  
│  ├─NCF  
│  │      NCF__Browsing_History__2021__epoch=100__lr=0.0005__l2=1e-07.txt  
│  │      NCF__Browsing_History__2021__epoch=10__lr=0.0005__l2=1e-07.txt  
│  │      NCF__Browsing_History__2021__epoch=1__lr=0.0005__l2=1e-07.txt  
│  │      NCF__Browsing_History__2021__epoch=2__lr=0.0005__l2=1e-07.txt  
│  │      NCF__Browsing_History__2021__epoch=30__lr=0.0005__l2=1e-07.txt  
│  │      NCF__Browsing_History__2021__epoch=5__lr=0.0005__l2=1e-07.txt  
│  │      NCF__RealRecord__2021__epoch=100__lr=0.0005__l2=1e-07.txt  
│  │
│  └─SASRec  
│          SASRec__RealRecord__2021__epoch=100__lr=0.0001__l2=1e-06.txt  
│
├─model  
└─src  
    │  exp.py  
    │  main.py  
    │  README.md  
    │  result_LOSS_HR_NDCG.py  
    │  run.sh  
    │  test.py  
    │ 
    ├─helpers  
    │  │  BaseReader.py  
    │  │  BaseRunner.py  
    │  │  __init__.py  
    │ 
    ├─models  
    │  │  BaseModel.py  
    │  │ 
    │  ├─general  
    │  │  │  NCF.py   
    │  │  │  __init__.py   
    │  │
    │  ├─sequential  
    │  │  │  SASRec.py  
    │  │  │  __init__.py  
    │ 
    └─utils  
        │  layers.py  
        │  utils.py  
        │  __init__.py  
	
