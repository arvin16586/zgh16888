markdown
```mermaid
graph TD
    %% ------------------- Style Definitions 优化 -------------------
    classDef source fill:#E5F5E0,stroke:#333,stroke-width:2px,color:#000;
    classDef integration fill:#D4EDF7,stroke:#333,stroke-width:2px,color:#000;
    classDef platform fill:#F8D7DA,stroke:#A4282D,stroke-width:3px,color:#000;
    classDef storage fill:#FFF3CD,stroke:#b38f00,stroke-width:2px,color:#000;
    classDef compute fill:#fbe4ff,stroke:#8e44ad,stroke-width:2px,color:#000;
    classDef smardaten fill:#cce5ff,stroke:#007bff,stroke-width:4px,color:#000,font-weight:bold;
    classDef dameng fill:#d1e7dd,stroke:#155724,stroke-width:4px,color:#000,font-weight:bold;
    classDef serving fill:#e2e3e5,stroke:#333,stroke-width:2px,color:#000;
    classDef app fill:#D6EAF8,stroke:#2E86C1,stroke-width:2px,color:#000;
    classDef governance fill:#f0e6ff,stroke:#563d7c,stroke-width:2px,color:#000;

    %% ------------------- 数据源层 -------------------
    subgraph A [数据源层 Data Sources]
        direction LR
        A1[风机传感器<br>SCADA]
        A2[光伏电站<br>逆变器]
        A3[核电站<br>DCS/SIS]
        A4[气象数据<br>API]
        A5[电网数据<br>SCADA]
        A6[运维数据<br>CMMS]
    end
    class A1,A2,A3,A4,A5,A6 source;

    %% ------------------- 数据集成层 -------------------
    subgraph B [数据集成层 Data Integration]
        B1[消息队列 Kafka<br>实时数据缓冲]
        B2[数据同步 Flume/DataX<br>离线数据采集]
    end
    class B1,B2 integration;

    %% ------------------- 大数据平台 包含存储与计算 -------------------
    subgraph P [MRS 大数据平台]
        subgraph C [统一存储层 HDFS]
            direction TB
            C1[ODS: 原始数据层]
            C2[DWD: 明细数据层]
            C3[DWS: 汇总数据层]
        end
        subgraph D [计算引擎层]
            direction LR
            D1[批处理 Spark]
            D2[流处理 Flink]
            D3[数据仓库 Hive]
        end
    end
    class P platform;
    class C,C1,C2,C3 storage;
    class D,D1,D2,D3 compute;

    %% ------------------- 数据服务与数仓层 -------------------
    subgraph S [数据服务与数仓层 Serving & Warehouse]
        S1[实时存储 HBase<br>设备状态/实时指标]
        S2[即席查询 Presto/Trino<br>Ad-hoc分析]
        subgraph DM[达梦数据库 DM8]
             DM1[ADS: 应用数据层]
             DM2[平台元数据<br>Hive Metastore/Smardaten Backend]
        end
    end
    class S1,S2 serving;
    %% ---- FIX: Removed spaces between node IDs ----
    class DM,DM1,DM2 dameng;

    %% ------------------- 敏捷数据中台 -------------------
    subgraph SM [Smardaten 敏捷数据中台 Workbench]
        direction TB
        SM1[数据开发与ETL<br>调度Spark/Flink作业]
        SM2[任务调度中心<br>工作流编排]
        SM3[数据可视化与BI<br>报表/驾驶舱]
    end
    class SM,SM1,SM2,SM3 smardaten;

    %% ------------------- 数据应用层 -------------------
    subgraph E [数据应用层 Applications]
        E1[实时监控驾驶舱]
        E2[发电量预测模型]
        E3[设备预测性维护]
        E4[运营分析报告]
        E5[碳足迹追踪]
    end
    class E1,E2,E3,E4,E5 app;
    
    %% ------------------- 平台管控与治理 贯穿始终 -------------------
    subgraph F [平台管控与治理 Governance & Ops]
        direction LR
        F1[安全<br>Ranger]
        F2[运维<br>MRS Manager]
    end
    class F1,F2 governance;

    %% ------------------- 数据流与控制流连接 -------------------
    %% 数据流入
    A -- "实时" --> B1
    A -- "离线" --> B2
    B1 --> C1
    B2 --> C1
    
    %% 平台内数据加工 Lambda架构
    B1 -- "实时消费" --> D2
    D2 -- "清洗/轻度聚合" --> C2
    D2 -- "实时状态写入" --> S1
    
    C1 -- "批处理读取" --> D1
    D1 -- "加工写入" --> C2
    C2 -- "汇总写入" --> C3
    C3 -- "聚合写入" --> DM1
    
    %% Smardaten的控制作用 虚线表示控制/编排
    SM1 -.->|开发与定义| D1
    SM1 -.->|开发与定义| D2
    SM2 -.->|调度执行| D1
    SM2 -.->|调度执行| D2
    
    %% 数据服务与应用
    S1 -- "查询" --> SM3
    S2 -- "查询" --> SM3
    DM1 -- "查询" --> SM3
    D3 -- "查询由Spark/Presto驱动" --> C;
    C -- "Ad-hoc查询" --> S2
    
    SM3 --> E1 & E4 & E5
    D1 -- "模型训练" --> E2
    S1 -- "实时特征" --> E3
    D2 -- "实时计算" --> E3

    %% 平台组件依赖关系 虚线
    DM2 -.->|元数据支持| D3
    DM2 -.->|后台数据库| SM
    F -.-> |全面管控| P
    F -.-> |全面管控| S
    F -.-> |全面管控| SM
```
