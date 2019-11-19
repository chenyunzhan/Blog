# 记录一次SQL查询为NULL的问题

## 背景描述

要求航班在起飞之后，不要自动触动延误的广播。所以在触动条件里加不等于起飞的条件，结果正常延误下也不触发广播了。
原因是：NULL值比较特殊，NULL不可以用!=符号比较
结论是：<font color=Red>!=无法查询出为Null的情况</font>


## 解决办法

修改前的SQL
```sql
SELECT *
  FROM DEPARTURE_DYNAMIC
 WHERE DF_STATUS_ABN_EXT_C = '延误'
   AND DF_ETIME IS NOT NULL
   AND DF_ATTR = 'D'
   AND DF_SHARETYPE != 'P'
   AND DF_STATUS_PRO_EXT_C != '起飞'
```
修改后的SQL
```sql
SELECT *
  FROM DEPARTURE_DYNAMIC
 WHERE DF_STATUS_ABN_EXT_C = '延误'
   AND DF_ETIME IS NOT NULL
   AND DF_ATTR = 'D'
   AND DF_SHARETYPE != 'P'
   AND (DF_STATUS_PRO_EXT_C != '起飞' OR DF_STATUS_PRO_EXT_C IS NULL)
```