---
title: patch
date: 2018-06-05 17:32:54
categories: ffcs
tags: 
    - patch
---

## 江苏的业务健康度大屏
### 主机负荷
1. 左侧
```sql
	  SELECT ROUND(AVG(A.CPU_USEDRATE), 2) CPU_USEDRATE,
		   ROUND(AVG(A.MEM_USEDRATE), 2) MEM_USEDRATE,
		   ROUND(AVG(A.IO_USEDRATE), 2) IO_USEDRATE,
		   ROUND(AVG(A.ORACLE_SESSION)) ORACLE_SESSION,
		   ROUND(AVG(A.ORACLE_PROCESS)) ORACLE_PROCESS,
		   ROUND(AVG(A.NET_DISCARD_RATE), 2) NET_DISCARD_RATE,
		   ROUND(AVG(A.NET_DELAY), 2) NET_DELAY,
		   MIN(A.NE_ID) DOMAIN_ID
	  FROM STAT_NE_BUSI_PERF A
	 WHERE NE_ID = 110505
```
2. 右侧
```sql
	    SELECT A.NE_ID,
           A.CONFIG_NAME,
           TO_CHAR(A.COLLECT_TIME, 'YYYY-MM-DD HH24:MI:SS') COLLECT_TIME,
           TO_CHAR(A.CREATE_TIME, 'YYYY-MM-DD HH24:MI:SS') CREATE_TIME,
           round(A.CPU_USEDRATE, 2) CPU_USEDRATE,
           round(A.MEM_USEDRATE, 2) MEM_USEDRATE,
           round(A.IO_USEDRATE, 2) IO_USEDRATE,
           A.ORACLE_SESSION,
           A.ORACLE_PROCESS,
           round(A.NET_DISCARD_RATE, 2) NET_DISCARD_RATE,
           A.NET_DELAY,
           B.SHORT_DESCRIPTION CI_NAME
      FROM STAT_NE_BUSI_PERF A,
           CI_BASE_ELEMENT B
     WHERE B.INSTANCE_ID = A.NE_ID(+)
     AND B.INSTANCE_ID in(110505,110506,110502,110503,110504,110501)
```
1.        {name: 'ITM', id: 110505}
2.        {name: '计费域', id: 110506}
3.        {name: 'OSS', id: 110502}
4.        {name: 'CRM', id: 110503}
5.        {name: 'EDA', id: 110504}
6.        {name: 'MSS', id: 110501}

### 中间区域数据
```sql
	  SELECT A.HOST_NUM      HOST_NUM,
		   A.DATA_BASE_NUM DATA_BASE_NUM,
		   A.NETWORK_NUM   NETWORK_NUM,
		   A.STORE_MEM     STORE_MEM,
		   A.CPU_CORES     CPU_CORES,
		   A.MEMORY        MEMORY,
		   A.IP_NUM        IP_NUM,
		   A.NE_ID         DOMAIN_ID
	  FROM STAT_NE_BUSI_CFG A
	 WHERE  A.BUSI_TYPE = 'DOMAIN'
     AND A.NE_ID in(110505,110506,110502,110503,110504,110501)
```
1.        {name: 'ITM', id: 110505}
2.        {name: '计费域', id: 110506}
3.        {name: 'OSS', id: 110502}
4.        {name: 'CRM', id: 110503}
5.        {name: 'EDA', id: 110504}
6.        {name: 'MSS', id: 110501}

### 整体配置,机房监控
```sql
 SELECT STAT_NE_BUSI_ID,
		   NE_ID,
		   CONFIG_NAME,
		   TO_CHAR(COLLECT_TIME, 'YYYY-MM-DD HH24:MI:SS') COLLECT_TIME,
		   TO_CHAR(CREATE_TIME, 'YYYY-MM-DD HH24:MI:SS') CREATE_TIME,
		   HOST_NUM,
		   DATA_BASE_NUM,
		   NETWORK_NUM,
		   STORE_MEM,
		   CPU_CORES,
		   MEMORY,
		   IP_NUM,
		   B.SHORT_DESCRIPTION CI_NAME
	  FROM STAT_NE_BUSI_CFG A,
		   CI_BASE_ELEMENT B
	 WHERE A.NE_ID = B.INSTANCE_ID
	   AND B.INSTANCE_ID in(58883438,58883439,42,41);
```
1.        {name: '二长', id: 58883438},
2.        {name: '鼓楼机房', id: 58883439},
3.        {name: '游府西街', id: 42},
4.        {name: '中央路', id: 41}

### 电能质量（近24 小时）
cardId查询 
```sql
SELECT T.INSTANCE_ID,T.SHORT_DESCRIPTION FROM CI_BASE_ELEMENT T WHERE T.CLASS_ID = 1420
```

```sql
SELECT T.* FROM T_ELE_ENERGY_HEALTH T WHERE T.INSTANCE_ID = #{cardId} order by t.ele_energy_health_id
```

### 实时告警

```sql 
SELECT * FROM SCREEN_ALARM_SHOW_INFO
```