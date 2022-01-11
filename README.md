### [Kaggle: Optiver Realized Volatility Prediction](https://www.kaggle.com/c/optiver-realized-volatility-prediction) ###
2021 Kaggle Featured Code Competition: Optiver Realized Volatility Prediction (Final ***Rank 94/3852*** teams)

![image](https://user-images.githubusercontent.com/57436423/148865267-93c72d43-6be2-490d-8afd-db1fe43ceb92.png)

### Solution: LightGBM + TabNet + MLP ###
- 比赛类型：结构化数据、时间序列比赛
- 竞赛任务：通过历史三个月不同行业数百支股票的短期波动率与财务数据来预测未来三个月真实市场每10分钟内的波动率。
- 模型：LightGBM + TabNet + MLP
- 评估指标：RMSPE

### 特征工程 ###
- 在相同time_id的不同时间窗口进行特征计算
- 将股票根据预测值进行聚类，计算每个聚类
- 计算每个time_id内的交易的平均时间间隔
- 基于成交价变动及成交量乘积构造tendency特征

### TabNet ###
Attentive Interpretable Tabular Learning（TabNet）由 Google Research 于 2019 年发布, 其本质是一种在表格数据上基于注意力机制来学习和做出预测的神经网络。它旨在结合树模型的可解释性和神经网络的高性能。
- **模型结构**
![image](https://user-images.githubusercontent.com/57436423/148870439-3d5f00a2-d91e-4d57-a85e-29aa74134ebb.png)

**TabNet编码器由feature transformer、attentive transformer和feature masking组成:**
- Feature transformer是一个多层网络（包括FC、BN和GRU），其中一些层将在每个步骤中共享，一些层独立处理。独立层和共享层的数量也是重要的超参数。
- 当特征经过预处理和转换后会传递到attentive transformer和feature masking中。 Attentive Transformer包括FC、BN和Sparsemax归一化，该模块会使用历史的尺度信息并在每次迭代中计算已经使用的特征数量，以此来专注于重要特征。 

**上分Trick**
- 剔除基于time_id进行agg函数特征构造可能导致的验证泄漏
- 引入了分层抽样 GroupKFold。限制验证集中仅出现新的时间 id（因为在预测时会出现这种情况），同时从每只股票中充分学习
- 多模型融合(LightGBM + TabNet + MLP)

通过市场历史3个月共126支不同行业股票的32.8k条短期波动率与相应财务数据来训练模型，***用未来3个月的实际市场结果来验证模型***（比赛在2021年9月27日截止提交，模型验证2021年9月28日-2022年1月11日的未来数据）

![image](https://user-images.githubusercontent.com/57436423/148865740-94c11193-ead3-478b-96c6-7a742776b9ae.png)

#### Final Rank: 94/3852 ####
![image](https://user-images.githubusercontent.com/57436423/148865967-dba789d9-35c5-43ec-b4c8-b32996bf7b3c.png)
第二次在Kaggle时序量化题中SOLO带队获得银牌
