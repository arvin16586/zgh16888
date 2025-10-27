markdown
```mermaid
graph TD
    %% ------------------- Style Definitions -------------------
    classDef domain fill:#E8F8F5,stroke:#16A085,stroke-width:2px;
    classDef flow fill:#FEF9E7,stroke:#F39C12,stroke-width:2px;
    classDef module fill:#EBF5FB,stroke:#3498DB,stroke-width:2px;
    classDef main_module fill:#D6EAF8,stroke:#2E86C1,stroke-width:3px,font-weight:bold;
    classDef db fill:#D5F5E3,stroke:#27AE60,stroke-width:3px,font-weight:bold;

    %% ------------------- 1. 数据域划分 (Data Domains) -------------------
    subgraph A [数据域]
        A1[<b>发电域</b><br><i>(Generation)</i><br>- 风机/光伏/核组<br>- 设备台账<br>- 实时工况]
        A2[<b>环境域</b><br><i>(Environment)</i><br>- 气象实况<br>- 气象预测<br>- 地理信息]
        A3[<b>电网域</b><br><i>(Grid)</i><br>- 负荷数据<br>- 电价信息<br>- 调度指令]
        A4[<b>运维域</b><br><i>(O&M)</i><br>- 巡检记录<br>- 维修工单<br>- 备品备件]
        A5[<b>运营域</b><br><i>(Operation)</i><br>- 成本效益<br>- 碳排放指标<br>- 交易合同]
    end
    class A1,A2,A3,A4,A5 domain;

    %% ------------------- 2. 逻辑数据流 (Logical Data Flow) -------------------
    subgraph B [逻辑数据流]
        direction LR
        B1[数据采集<br><i>(Collection)</i><br>实时/批量] --> B2[数据缓冲<br><i>(Buffering)</i><br>消息队列]
        B2 --> B3[数据存储与分层<br><i>(Storage & Layering)</i><br>ODS->DWD->DWS]
        B3 --> B4[数据计算<br><i>(Computation)</i><br>流式/批式/交互式]
        B4 --> B5[数据服务<br><i>(Serving)</i><br>API/查询服务/模型服务]
    end
    class B1,B2,B3,B4,B5 flow;

    %% ------------------- 3. 核心功能模块 (Core Functional Modules) -------------------
    subgraph C [核心功能模块]
        C1[<b>数据集成与开发模块</b><br><i>(Smardaten)</i><br>- 数据源管理<br>- 图形化ETL<br>- 代码化开发(SQL/Python)]
        C2[<b>任务调度模块</b><br><i>(Smardaten)</i><br>- 工作流编排<br>- 依赖管理<br>- 监控告警]
        C3[<b>统一计算引擎模块</b><br><i>(MRS)</i><br>- 分布式批处理<br>- 分布式流处理]
        C4[<b>统一存储模块</b><br><i>(MRS)</i><br>- 分布式文件系统<br>- KV存储]
        C5[<b>关系型数仓模块</b><br><i>(达梦数据库)</i><br>- 聚合结果存储(ADS)<br>- 多维分析支持<br>- 平台元数据管理]
        C6[<b>数据服务与可视化模块</b><br><i>(Smardaten)</i><br>- 数据API生成<br>- BI报表与大屏<br>- 数据目录与血缘]
    end
    class C1,C2,C6 main_module;
    class C3,C4 module;
    class C5 db;

    %% ------------------- 关系连接 -------------------
    A -- "作为输入" --> B1
    C1 -- "定义" --> B4
    C2 -- "驱动" --> B4
    C3 -- "执行" --> B4
    B3 -- "被...读写" -- C4
    B4 -- "结果写入" --> B3
    B4 -- "结果写入" --> C5
    C5 -- "提供元数据给" --> C3 & C1
    B5 -- "由...提供" --> C6
    C5 -- "被...查询" --> C6
    C4 -- "被...查询" --> C6
```
