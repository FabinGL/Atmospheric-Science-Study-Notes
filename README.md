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

在第一阶段的工作中，使用的是ERA5的数据集，已经完成了初步的数据清洗和模型调整，最后所使用的数据如下表所示：
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
