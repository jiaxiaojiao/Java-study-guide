# SQL获取指定时间

- 今天零点

```sql
SELECT CAST(CAST(SYSDATE()AS DATE)AS DATETIME)

```

- 昨天零点

```text
SELECT CAST((CAST(SYSDATE()AS DATE) - INTERVAL 1 DAY)AS DATETIME)

```