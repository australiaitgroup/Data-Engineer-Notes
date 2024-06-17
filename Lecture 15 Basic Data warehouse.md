# Basic Data Warehouse

## 目录

- [图示讲解什么是数据仓库 (Data Warehouse)](#图示讲解什么是数据仓库-data-warehouse)
  - [基础概念](#基础概念)
- [Dimension Table 技术](#dimension-table-技术)
- [Fact Table 技术](#fact-table-技术)
- [Integration via Conformed Dimensions](#integration-via-conformed-dimensions)
- [SCD: Slowly Changing Dimension](#scd-slowly-changing-dimension)
- [总结](#总结)

## 什么是 Data Warehouse

数据仓库 (Data Warehouse) 是用于存储和管理大量历史数据的系统，这些数据通常从多个来源收集，用于分析和报告。数据仓库的设计有助于支持业务决策，通过组织和整合数据，使其易于访问和分析。

### 基础概念

1. **Gathering Business Requirements**

   - 与业务专家合作，了解并收集业务需求，明确数据仓库的目标。

2. **Designing the Dimensional Model**

   - 通过协作的维度建模研讨会，与团队中的业务专家一起设计维度模型。

3. **4 Four Steps of Dimensional Design**
   - Identify Business Processes：确定数据仓库要处理的核心业务过程。
   - Declare the Grain：决定数据仓库的详细程度，尽量保持最低粒度。在选择维度或事实之前必须声明粒度。
   - Identify Dimensions：确定 who, what, where, when, why 和 how 等维度。
   - Identify Facts：确定要捕获和存储的事实数据。

## Dimension Table 技术

1. **Dimension Table 结构**

   - 通常是宽的、扁平的非规范化表，具有许多低基数的文本属性。

2. **Dimension Surrogate Keys**

   - 使用简单的整数作为替代键，从 1 开始，每次需要新键时按顺序分配。
   - 日期维度可以使用更有意义的主键，而不使用替代键。

3. **Durable Key**

   - 在替代键可能更改时使用持久键。

4. **Drill Down**

   - 通过 Drill Down 操作进一步细化数据查看。

5. **Degenerate Dimensions**

   - 简化维度是一种非规范化维度，通常包含来自事实表的事务标识符。

6. **Denormalized Flattened Dimensions**

   - 扁平化维度将多层次的层级结构合并到一个表中，以提高查询性能。

7. **Multiple Hierarchies**

   - 维度表中可能包含多个层级结构，以便支持不同的聚合级别。

8. **Flags and Indicators**

   - 使用标志和指示器进行数据标记，便于数据分类和筛选。

9. **Null Attributes in Dimension**

   - 使用 "unk" 或 "N/A" 替代 Null，以避免查询中的空值问题。

10. **Calendar Table**

    - 专用的日历表用于日期维度，便于日期计算和时间分析。

11. **Role Playing Dimensions**

    - 角色扮演维度是同一个维度表在不同上下文中的不同角色，如订单日期和发货日期。

12. **Junk Dimension**

    - 将多个低基数的属性组合成一个混杂维度，以减少维度表的数量。

13. **Snowflaked Dimension**

    - 通过规范化处理的维度表，其中每个层级的属性都存储在单独的表中。

14. **Outrigger Dimensions**

    - 外挂维度是一个辅助维度表，用于存储与主要维度表相关的属性。

## Fact Table 技术

1. **Fact Table 结构**

   - 事实表包含数值型的度量数据，通常是一个窄表，具有高基数的属性。

2. **Additive, Semi-additive, Non-additive Facts**

   - **Additive Facts**: 可以跨所有维度进行加总的事实数据，如销售金额。
   - **Semi-additive Facts**: 只能跨某些维度进行加总的事实数据，如期末余额。
   - **Non-additive Facts**: 不能进行加总的事实数据，如比率和百分比。

3. **Nulls in Fact Tables**

   - 使用 "unk" 或 "N/A" 替代 Null，以避免查询中的空值问题。

4. **Conformed Table**

   - 一致表是多个数据集市之间共享的维度表，以确保数据的一致性和整合性。

5. **Transaction Fact Tables**

   - 事务事实表记录单个业务事件的详细信息，如销售交易。

6. **Periodic Snapshot Fact Tables**

   - 定期快照事实表捕捉在固定时间间隔内的度量数据，如每月销售额。

7. **Accumulating Snapshot Fact Table**

   - 累积快照事实表跟踪一个业务过程的所有阶段，如订单处理的各个步骤。

8. **Factless Fact Tables**

   - 无事实事实表仅包含维度键，用于跟踪事件或状态的存在，如学生出勤记录。

9. **Aggregated Fact Tables**

   - 聚合事实表包含预计算的汇总数据，以提高查询性能。

10. **Consolidated Fact Tables**

    - 综合事实表包含来自多个数据源的合并数据，用于提供全局视图。

## Integration via Conformed Dimensions

- 通过一致维度进行数据集成，确保不同数据集市之间的数据一致性和可比较性。

## SCD: Slowly Changing Dimension

1. **Type 1: No History Preservation**

   - 直接覆盖旧值，不保留历史记录，适用于少量变化的数据。

2. **Type 2: Preserve History**

   - 添加新的行以保留历史记录，使用替代键、有效日期和过期日期来标识数据版本。

3. **Type 3: Partially Preserve History**

   - 在现有行中添加新的列来存储旧值，保留有限的历史信息。

## 总结

本次课程深入探讨了数据仓库的基本概念和技术，包括 Kimball 方法、维度表和事实表的设计及其技术细节。通过学习如何使用一致维度进行数据集成，以及处理慢变化维度 (SCD) 的不同方法，学生将能够理解并应用这些关键概念和技术。
