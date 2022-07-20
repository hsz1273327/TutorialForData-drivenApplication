* [回到博客](http://blog.hszofficial.site/)
* [数据驱动应用开发攻略](README.md)
* [应用的渐进式架构]
* [顶层设计与数据管理]
    * [数据规模与数据管理](数据管理篇/数据规模与数据管理.md)
    * [元数据管理](数据管理篇/元数据管理.md)
    * [数据的标准化](数据管理篇/数据的标准化.md)
    * [数据的生命周期](数据管理篇/数据的生命周期.md)
    * [数据分层](数据管理篇/数据分层.md)
    * [数据的监控](数据管理篇/数据的监控.md)

* [数据生产](数据生产篇/README.md)
    * [埋点数据采集](数据生产篇/埋点数据采集.md)
    * [日志数据采集](数据生产篇/日志数据采集.md)
    * [外部数据采集](数据生产篇/外部数据采集.md)

* [数据存储篇]
    * 数据存储的生命周期与层次
    * 业务数据存储
    * 分析数据存储
    * 报表存储
    * 特征离线存储
    * 特征在线存储

* 数据清洗篇
    * 静态数据清洗
    * 流数据清洗

* 数据分析篇
    * [数据可视化篇](数据可视化篇/README.md)
        * 用于分析过程中的可视化
        * 用于展示结论的可视化
        * 可视化服务

    * [数据挖掘篇](数据可视化篇/README.md)

* [机器学习应用落地](机器学习应用落地/README.md)

* 附录1:工具介绍
    * 协议
        * [JSON](数据管理篇/工具介绍/JSON.md)
        * [JSONSchema](数据管理篇/工具介绍/JSONSchema.md)
        * [Protobuf](数据管理篇/工具介绍/Protobuf.md)
    * 交互层组件
    * 业务层组件
    * [存储层组件](工具介绍/存储层组件/README.md)
        * [REDIS](工具介绍/存储层组件/REDIS.md)
        * [ClickHouse]()
        * [S3和minio]()
    * 推理层组件
        * [feast]()
        * streamz
            * [流计算](数据清洗篇/工具介绍/streamz/流计算/流计算.md)
        * pyflink
    * 分析层组件
        * [pandas](数据清洗篇/工具介绍/pandas/README.md)
            * [pandas的序列对象](数据清洗篇/工具介绍/pandas/pandas的序列对象.md)
            * [pandas的数据框对象](数据清洗篇/工具介绍/pandas/pandas的数据框对象.md)
            * [复合索引](数据清洗篇/工具介绍/pandas/复合索引.md)
            * [分组与集聚](数据清洗篇/工具介绍/pandas/分组与集聚.md)
            * [pandas的函数操作](数据清洗篇/工具介绍/pandas/pandas的函数操作.md)
            * [基本的统计分析](数据清洗篇/工具介绍/pandas/基本的统计分析.md)
            * [时间序列](数据清洗篇/工具介绍/pandas/时间序列/时间序列.md)
            * [数据获取与保存](数据清洗篇/工具介绍/pandas/数据获取与保存.md)
            * [数据可视化](数据清洗篇/工具介绍/pandas/数据可视化/数据可视化.md)
        * [dask](数据清洗篇/工具介绍/dask/README.md)
            * [分布式数据结构](数据清洗篇/工具介绍/dask/分布式数据结构.md)
            * [dask作为算力池](数据清洗篇/工具介绍/dask/dask作为算力池.md)
            * [实时任务提交](数据清洗篇/工具介绍/dask/实时任务提交.md)
        * pyspark
        * [SymPy](数据分析篇/工具介绍/SymPy/README.md)
            * [声明符号和内置符号类型](数据分析篇/工具介绍/SymPy/声明符号和内置符号类型.md)
            * [符号计算](数据分析篇/工具介绍/SymPy/符号计算/README.md)
                * [表达式操作](数据分析篇/工具介绍/SymPy/符号计算/表达式操作/表达式操作.md)
                * [微积分](数据分析篇/工具介绍/SymPy/符号计算/微积分/微积分.md)
                * [解方程](数据分析篇/工具介绍/SymPy/符号计算/解方程/解方程.md)
                * [线性代数](数据分析篇/工具介绍/SymPy/符号计算/线性代数/线性代数.md)
                * [概率统计](数据分析篇/工具介绍/SymPy/符号计算/概率统计/概率统计.md)
                * [布尔代数](数据分析篇/工具介绍/SymPy/符号计算/布尔代数/布尔代数.md)
            * [输出](数据分析篇/工具介绍/SymPy/输出/README.md)
                * [求值](数据分析篇/工具介绍/SymPy/输出/求值.md)
                * [更加优雅的输出打印结果](数据分析篇/工具介绍/SymPy/输出/更加优雅的输出打印结果/更加优雅的输出打印结果.md)
                * [画图](数据分析篇/工具介绍/SymPy/输出/画图/画图.md)
        * [python标准库中的计算工具](数据分析篇/工具介绍/python标准库中的计算工具/使用标准库处理基本数学问题.md)
        * [numpy和scipy](数据分析篇/工具介绍/numpy和scipy/README.md)
            * [numpy的高性能同构定长多维数组](数据分析篇/工具介绍/numpy和scipy/numpy的高性能同构定长多维数组/numpy的高性能同构定长多维数组.md)
            * [universal_function](数据分析篇/工具介绍/numpy和scipy/universal_function/universal_function.md)
            * [精度与基本数学运算](数据分析篇/工具介绍/numpy和scipy/精度与基本数学运算.md)
            * [多项式计算](数据分析篇/工具介绍/numpy和scipy/多项式计算/多项式计算.md)
            * [线性代数](数据分析篇/工具介绍/numpy和scipy/线性代数.md)
            * [统计计算](数据分析篇/工具介绍/numpy和scipy/统计计算/统计计算.md)
            * [傅里叶变换](数据分析篇/工具介绍/numpy和scipy/傅里叶变换/傅里叶变换.md)
            * [窗口函数与卷积](数据分析篇/工具介绍/numpy和scipy/窗口函数与卷积/窗口函数与卷积.md)
            * [财务分析](数据分析篇/工具介绍/numpy和scipy/财务分析.md)
        * [matplotlib](数据可视化篇/工具介绍/matplotlib/README.md)
            * [matplotlib的基本设置](数据可视化篇/工具介绍/matplotlib/matplotlib的基本设置/matplotlib的基本设置.md)
            * [绘图工具pyplot](数据可视化篇/工具介绍/matplotlib/绘图工具pyplot/绘图工具pyplot.md)
            * [非结构网络](数据可视化篇/工具介绍/matplotlib/非结构网络/非结构网络.md)
            * [桑基图](数据可视化篇/工具介绍/matplotlib/桑基图/桑基图.md)
            * [图片加载](数据可视化篇/工具介绍/matplotlib/图片加载/图片加载.md)
            * [保存图片](数据可视化篇/工具介绍/matplotlib/保存图片/保存图片.md)
            * [控件](数据可视化篇/工具介绍/matplotlib/控件.md)
            * [动画](数据可视化篇/工具介绍/matplotlib/动画/动画.md)
            * [matplitlib结合web技术](数据可视化篇/工具介绍/matplotlib/matplitlib结合web技术.md)
            * [结语](数据可视化篇/工具介绍/matplotlib/结语.md)
    * 建模层组件
    * CI/CD层组件
    * 监控层组件
* [附录2:术语表](术语表/README.md)
    * [统计](术语表/统计/README.md)
        * [误差](术语表/统计/误差.md)
    * [信息论](术语表/信息论/README.md)
        * [信息空间](术语表/信息论/信息空间.md)
    * [网络](术语表/网络/README.md)
        * [URI与URL](术语表/网络/URI与URL.md)