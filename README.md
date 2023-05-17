# Atmospheric Science Study Notes💦

我现在任职于[三维时空股份有限公司](http://xn--ehq269cgqo6ge.xn--fiqs8s/)从事大气学科与机器学习交叉学科的项目。我会以此博客记录工作中遇到的一些问题与目前自己的学习记录。

## 晴雨预报与暴雨预报

晴雨预报与降雨预报是第一阶段的工作，主要是负责短期预报形式。以天为单位，目标是判断隔天有无降雨以及是否有特大暴雨的情况，即：

**假设一个城市$C$某天$n$的降水总量(Total Precipitation)为$T_p$,那么则有**

\begin{equation}
\left\{
             \begin{array}{lr}
             \mbox{$C$城市第$n$天将没有降雨;} & T_p \leq 0.1$mm$ \\
             \mbox{$C$城市第$n$天将会有降雨;} & T_p > 0.1$mm$ \\
             \mbox{$C$城市第$n$天将有大暴雨.} & T_p > 32$mm$  
             \end{array}
\right.
\end{equation}

晴雨预报与暴雨短时预报的[报告](Rainfall_Report20230427.pdf)
