markdown
```mermaid
graph TD
    A[报告基本信息表 (id(report_id))] --> B[报告检查范围表 (report_id(综合报告ID),  id(部件ID))];
    B --> C[报告检查范围明细表 (id(item_id))];
    A --> D[报告仪器设备信息表 (report_id)];
    A --> E[报告附件清单 (report_id)];


    A[记录单基本信息表 (id(record_id))] --> B[检查范围表 (record_id(原始记录单ID), id(部件ID))];
    A --> C[技术条件表 (record_id)];
    B --> D[照片表 (item_id(部件ID))];
    A --> E[班组成员表 (record_id)];
```
