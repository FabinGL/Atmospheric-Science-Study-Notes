# Atmospheric Science Study Notes💦

我现在任职于[三维时空股份有限公司](http://xn--ehq269cgqo6ge.xn--fiqs8s/)从事大气学科与机器学习交叉学科的项目。我会以此博客记录工作中遇到的一些问题与目前自己的学习记录。

## 晴雨预报与暴雨预报

晴雨预报与降雨预报是第一阶段的工作，主要是负责短期预报形式。以天为单位，目标是判断隔天有无降雨以及是否有特大暴雨的情况，即：

**假设一个城市 $C$ 某天 $n$ 的降水总量(Total Precipitation)为 $T_p$ ,那么则有**

$$
\left\{
             \begin{array}{lr}
             \mbox{$C$城市第$n$天将没有降雨;} & T_p \leq 0.1 mm \\
             \mbox{$C$城市第$n$天将会有降雨;} & T_p > 0.1 mm \\
             \mbox{$C$城市第$n$天将有大暴雨.} & T_p > 32 mm  
             \end{array}
\right.
$$

在第一阶段的工作中，使用的是ERA5的数据集，已经完成了初步的数据清洗和模型调整。

我尝试了多种模型的结果，目前的预测模式是前七天迭代预测第八天的降雨情况，使用的是TS评分，分别使用了单特征、前五个特征和前八个特征。这本质上是两个分类问题。所以使用的模型均为分类模型。

特大暴雨结果如下：
| Machine Learning | Number of features |               |
|------------------|--------------------|---------------|
|                  | Five characteristics | Eight characteristics |
| Logistic | 0.02 | 0.02 |
| SVC | 0 | 0 |
| BLS | 0 | 0 |
| DT | **0.08** | **0.07** |
| RF | 0.04 | 0.06 |
| GBDT | 0.04 | 0.04 |
| XGBoost | 0.03 | 0.03 |
| Etr | 0 | 0 |
| Lgb | 0.04 | 0.04 |

晴雨预报结果如下:
| Machine Learning      | Number of features |       |       | Deep Learning/VMD-ML  |     |
|-----------------------|:------------------:|:-----:|:-----:|:----------------------:|:---:|
|                       | One                | Five  | Eight |                        | One |
| Logistic              | 0.671              | 0.703 | 0.694 | RNN                    | 0.651 |
| SVC                   | 0.672              | 0.693 | 0.699 | LSTM                   | **0.668** |
| BLS                   | 0.667              | 0.672 | 0.669 | GRU                    | 0.664 |
| DT                    | 0.624              | 0.672 | 0.682 | LSTNet                 | 0.653 |
| RF                    | 0.702              | 0.714 | 0.711 |                        |      |
| GBDT                  | **0.719**          | **0.731** | **0.729** | VMD-GBDT  | 0.718 |
| XGBoost               | 0.709              | 0.722 | 0.714 |                        |      |
| Etr                   | 0.710              | 0.717 | 0.712 |
| Lgb                   | 0.714              | 0.722 | 0.724 |                        |      |

单站点调参的结果如下：
|                                          | *Machine Learning* |      **The target variable**       |
| :--------------------------------------: | :----------------: | :--------------------------------: |
|                 **Five**                 |        GBDT        |      0.77($Iterative$)      |         $None$         |
|                                          |         DT         |          $None$           |           0.141            |
|                **Eight**                 |        GBDT        |      0.75($Iterative$)      |         $None$         |
|                                          |         DT         |          $None$           |          0.129           |







***具体了解请阅读晴雨预报与暴雨短时预报的[报告](Rainfall_Report20230427.pdf)。***

--- 

## 月季度的降水预报

第二部分的工作我将会负责月季度降水量的工作，使用的仍然是ERA5的福建地区的数据，数据精度为0.25 $km$ \* 0.25 $km$ 。目的是预测次月的月降水总量，是一个回归的问题。最后清洗的表格为19\*20=380条序列，就是每个月一个格点会有一条序列，那么每个月就会有380条序列数据。

### 数据集整合

在第一部分的工作中，由于数据清洗量庞大，所以我没有用到数据集中所有的特征。但是在本次实验中，所需要使用的特征已经清洗完成。使用的特征数量如下所示：
| **Feature**                                   | **Variable Name** | **Introduction**                                                                                                     |
| --------------------------------------------- | ----------------- | -------------------------------------------------------------------------------------------------------------------- |
| <font color="#FE0000">Total Precipitation</font>     | <font color="#FE0000">\$T\_p\$</font>   | <font color="#FE0000">Total precipitation in the 24 hours</font>                                                        |
| <font color="#3166FF">Convective Precipitation</font> | <font color="#3166FF">\$C\_p\$</font>   | <font color="#3166FF">Precipitation from convective clouds</font>                                                    |
| <font color="#3166FF">10m v Component of Wind</font>  | <font color="#3166FF">\$W\_{cv}\$</font> | <font color="#3166FF">10m v component of wind</font>                                                                 |
| <font color="#3166FF">10m u Component of Wind</font>  | <font color="#3166FF">\$W\_{cu}\$</font> | <font color="#3166FF">10m u component of wind</font>                                                                 |
| <font color="#3166FF">2m Temperature</font>           | <font color="#3166FF">\$T\_{2m}\$</font> | <font color="#3166FF">Temperature at 2m height</font>                                                               |
| <font color="#3166FF">Temperature</font>              | <font color="#3166FF">\$T\$</font>      | <font color="#3166FF">Daily temperature</font>                                                                     |
| <font color="#3166FF">Relative Humidity</font>        | <font color="#3166FF">\$H\_r\$</font>   | <font color="#3166FF">Relative humidity, the percentage of water vapor pressure to water saturated vapor pressure</font> |
| <font color="#3166FF">Specific Humidity</font>        | <font color="#3166FF">\$H\_s\$</font>   | <font color="#3166FF">Specific humidity, the ratio of the mass of water vapor to the total mass of air in the parcel</font> |
| Vorticity                                      | \$V\$            | Vorticity, used to describe the rotational state of the fluid                                                         |
| Divergence                                     | \$D\$            | Divergence, the horizontal divergence of velocity                                                                      |
| Geopotential                                   | \$G\$            | Geopotential, refers to the undulating and scaling of the surface in horizontal and vertical planes                    |
| Large scale precipitaiton                      | \$P\_s\$         | Large-scale precipitation                                                                                             |
| Mean top net long wave radiation flux          | \$F\_r\$         | Mean top net longwave radiation flux                                                                                   |
| Potential vorticity                            | \$P\_v\$         | Potential vorticity                                                                                                   |
| Surface latent heat flux                       | \$F\_h\$         | Surface latent heat flux                                                                                              |
| Surface net solar radiation                    | \$R\_s\$         | Net surface solar radiation                                                                                           |
| Surface net solar radiation clear sky          | \$R\_{sk}\$      | Net surface solar radiation under clear sky                                                                           |
| Surface pressure                               | \$P\$            | Surface pressure                                                                                                      | 

这里的特征为与ERA5数据中的原始存在的特征。后面还要添加海洋活动数据和太阳黑子数据，同时也会添加一些别的地区的数据（**与福建遥相关的地区,后面会具体解释遥相关的概念**）。同时对于已经清洗的数据要做一个时间上的数据对齐。

对于以上的数据我们做了一个每日降水量的数据修正，假设每日降水量为 $T_p$，则有：

$$
T_p < 0  \Rightarrow T_p = 0
$$

模型的输入特征如表格所示，假设某个城市其中一天$t$的数据的第一个特征为$X^1(t)$,那么其输入模型的数据矩阵$A$:其中模型的输入为前$n$月的特征值，模型的输出则为第$n+1$月的站点一月总降水量(这里$n$的取值为7)，即为矩阵$B.T$(矩阵$B$的转置)。数据集采集为迭代采集，采集方法如图所示。

![s_windows](image/S_windows.png)

$$
	A=\begin{bmatrix}
	x_1^1(t)&X_1^2(t)&\cdots &X_1^7(t+7)  \\
	X_2^1(t+1) &X_2^2(t+1) &\cdots  &X_2^7(t+8) \\
	\vdots  & \vdots  &\ddots   &\vdots\\
    X_{n+7}^1(t+n)& X_{n+7}^2(t+n) &\cdots  &X_{n+7}^7(t+n+7)
	 \end{bmatrix}
$$



$$
	B=\begin{bmatrix}
	  y_{t+1}&y_{t+2}&\cdots &y_{t+n}  
	 \end{bmatrix}
$$

#### 遥相关的概念
当我们研究气象现象时，往往需要考虑到空间间隔和时间间隔对这些现象的影响。而遥相关则是用来描述这种空间间隔对气象现象的影响。

具体来说，遥相关指的是位于不同空间点的相似气象现象之间的关系，也就是说，当两个空间点之间的距离越近，它们的气象现象越相似。而这种现象的相似程度可以用相关系数来表示。对于降雨来说，遥相关可以用来描述不同地区的降雨量之间的关系。通常情况下，离得近的地区的降雨量会更加相似，而离得远的地区的降雨量可能差别很大。因此，在研究降雨时，遥相关可以帮助我们理解和预测不同地区的降雨量之间的关系。
