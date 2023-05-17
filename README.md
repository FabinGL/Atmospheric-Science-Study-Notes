# Atmospheric Science Study Notes💦

我现在任职于[三维时空股份有限公司](http://xn--ehq269cgqo6ge.xn--fiqs8s/)从事大气学科与机器学习交叉学科的项目。我会以此博客记录工作中遇到的一些问题与目前自己的学习记录。

## 晴雨预报与暴雨预报

晴雨预报与降雨预报是第一阶段的工作，主要是负责短期预报形式。以天为单位，目标是判断隔天有无降雨以及是否有特大暴雨的情况，即：

**假设一个城市 $C$ 某天 $n$ 的降水总量(Total Precipitation)为 $T_p$ ,那么则有**

$$
\left\{
             \begin{array}{lr}
             \mbox{$C$城市第$n$天将没有降雨;} & T_p \leq 0.1$mm$ \\
             \mbox{$C$城市第$n$天将会有降雨;} & T_p > 0.1$mm$ \\
             \mbox{$C$城市第$n$天将有大暴雨.} & T_p > 32$mm$  
             \end{array}
\right.
$$

在第一阶段的工作中，已经完成了初步的数据清洗和模型调整，最后所使用的数据如下表所示：
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




晴雨预报与暴雨短时预报的[报告](Rainfall_Report20230427.pdf)
