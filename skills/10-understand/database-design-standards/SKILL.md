---
name: database-design-standards
description: 数据库设计规范：表/索引/SQL/变更。Use when 设计表结构、编写 SQL、做 DDL 变更时必读。Do not use when 纯应用层逻辑开发。
---

# Database Design Standards

新增/修改 SQL（含 ORM、工具实际生成 SQL）必须逐条执行 [SQL 性能验收](../../../docs/sql-performance-validation.md) 的“精简与效率复检”，比较最小等价索引路径，再做预执行与长期性能验收；参数或执行方式变化后重检。不以工具默认写法、“有索引”或平均耗时低代替这一复检。

## 表设计

**命名**：全小写+下划线，长度<=30，布尔加 `is_`，外键 `被引用表_id`，禁用保留字

**主键**：`BIGINT UNSIGNED AUTO_INCREMENT`，字段名 `id`，禁止业务字段作主键/UUID

**字段类型优先级**：数字 > 日期 > 字符串
- 精度高用 DECIMAL(m,n)，否则 DOUBLE
- 固定长 CHAR，可变长 VARCHAR（必须限长）
- 禁止 TEXT 在常查询列

**必备字段**
- `created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP`
- `updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP`
- 状态字段必须 DEFAULT，计数字段 DEFAULT 0

## 索引设计

**何时加**：WHERE/JOIN ON/ORDER BY/GROUP BY 字段，选择性>=60%

**联合索引**
- 最左前缀原则，严格条件在前
- 范围条件（>, <, BETWEEN, LIKE）后字段无法用索引
- 单表索引<=5 个

**覆盖索引**：将 SELECT 字段包含在索引中避免回表，仅对高频查询使用

## SQL 编写

| 禁止 | 替代 |
|------|------|
| `SELECT *` | 明确列出字段 |
| `WHERE DATE(col)=...` | `WHERE col BETWEEN ...` |
| 隐式类型转换 `WHERE id='123'` | `WHERE id=123` |
| `IN (SELECT ...)` 大结果集 | 改 JOIN |
| `LIMIT 100000, 20` | ID 游标 `WHERE id > last_id LIMIT 20` |
| 字符串拼接 SQL | PreparedStatement |

**JOIN 规范**：优先 INNER JOIN，最多 3 表，JOIN 字段必须有索引且类型一致，小表驱动大表

## 数据完整性

- 核心字段 NOT NULL + DEFAULT
- 禁止用 NULL 表示逻辑状态，用显式状态码
- 业务数据逻辑删除（`deleted_at`），辅助数据可级联删除
- 高并发环境外键约束改应用层+定期校验

## 表结构变更

先执行 [变更范围与改表方案审核](../../../docs/change-scope-schema-review.md)：在正式代码依赖新结构前，由独立子代理核对变更必要性、免改表替代方案、真实表规模、执行影响、兼容与恢复；实质扩大范围或新增风险须先获得具体方案确认。不能先加字段再把改表作为发布前提。

**向后兼容三步法（无停机）**

```
发布1: 新增字段（有 DEFAULT，无 NOT NULL）→ 应用写新旧两字段
发布2: 数据迁移 → 应用只读写新字段
发布3: 删除旧字段
```

- 字段改名 → 禁止直接改，走新增+迁移+删除
- 字段扩容 → 检查应用无上限假设
- 大表迁移 → 新表双写+增量迁移+校验切换

## 反模式

| 模式 | 风险 | 替代 |
|------|------|------|
| EAV（实体-属性-值） | 查询复杂、JOIN 多 | JSON 字段+应用层解析 |
| 多态关联（type + id） | 无法数据库约束 | 独立外键表 |
| 过度范式化 | JOIN 过多 | 高频只读场景可反范式 |

原则：优先范式化，性能瓶颈后反范式化，反范式需配更新策略。

## AI 常见陷阱

| 陷阱 | 正确做法 |
|------|---------|
| 生成已存在表/索引 | 加 IF NOT EXISTS |
| 新增字段加 NOT NULL 无 DEFAULT | 必须带 DEFAULT |
| 过多索引（10+） | 单表<=5 个 |
| LIKE '%keyword%' 期望走索引 | 前缀匹配才能用索引 |
| 批量 INSERT 无事务 | BEGIN/COMMIT 包裹 |
| NULL 作默认状态 | 显式状态码+NOT NULL |
