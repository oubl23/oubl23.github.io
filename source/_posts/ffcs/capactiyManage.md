---
title: patch
date: 2018-06-14 15:28:54
categories: ffcs
tags: 
    - patch
---

## 容量管理
### 左侧菜单
```sql
     SELECT T.*,
      LEVEL S_LEVEL,
      SYS_CONNECT_BY_PATH(T.MENU_ID,',') PATH,
      (SELECT COUNT(*)
         FROM SIDEBAR_MENU_CFG T2
        WHERE T2.MENU_PARENT_ID = T.MENU_ID) AS IS_PARENT
     FROM SIDEBAR_MENU_CFG T
    WHERE T.MENU_TYPE = 'capacityManage'
    AND STATE = '0SA'
   CONNECT BY PRIOR T.MENU_ID = T.MENU_PARENT_ID
    START WITH T.MENU_PARENT_ID IS NULL
    ORDER SIBLINGS BY T.ORDER_NUM
```
### 概览
```sql
        SELECT DISTINCT A.INSTANCE_ID          HOST_ID,
            A.CLASS_ID             HOST_CLASS_ID,
            A.SHORT_DESCRIPTION    HOST_NAME,
            BS.BUSI_DOMAIN_ID,
            BS.BUSI_DOMAIN_NAME,
            B.INSTANCE_ID          DEST_ID,
            B.CLASS_ID             DEST_CLASS_ID,
            B.SHORT_DESCRIPTION    DEST_NAME
            FROM CI_BASE_ELEMENT A,
            CI_BASE_ELEMENT B,
            CI_BASE_RELATIONSHIP C,
            CI_BASE_ELEMENT BUSI,
            CI_BASE_RELATIONSHIP BUSI_REL,
            (SELECT B.CI_CLASS_ID BUSI_CLASS_ID,
                A.TREE_LABEL  BUSI_TYPE_NAME,
                C.TREE_ID     BUSI_DOMAIN_ID,
                C.TREE_LABEL  BUSI_DOMAIN_NAME
                FROM BASIC_TREE_NODE A, CI_SYS_VIEW_TREE B, BASIC_TREE_NODE C
                WHERE A.TREE_ID = B.TREE_ID
                AND A.P_TREE_ID = C.TREE_ID
                AND A.PUBLIC_TREE_KEY = 'CI_BUSI_TREE_FOR_CI_VIEW'
           
                AND C.TREE_ID = #{paramMap.domainId}
            ) BS
            WHERE A.INSTANCE_ID = C.SOURCE_INSTANCE_ID
            AND B.INSTANCE_ID = C.DESTINATION_INSTANCE_ID
            AND A.INSTANCE_ID = BUSI_REL.SOURCE_INSTANCE_ID
            AND BUSI.INSTANCE_ID = BUSI_REL.DESTINATION_INSTANCE_ID
            AND BUSI.CLASS_ID = BS.BUSI_CLASS_ID
            AND A.DATASET_ID = 6
            AND A.MARKASDELETED = 0
            AND B.DATASET_ID = 6
            AND B.MARKASDELETED = 0
            AND C.MARKASDELETED = 0
            AND BUSI.MARKASDELETED = 0
            AND BUSI.DATASET_ID = 6
            AND A.CLASS_ID IN
            (SELECT CLASS_ID FROM CI_CLASS_TREE WHERE SUPER_CLASS_ID = 118)
            AND BUSI.CLASS_ID IN (SELECT CLASS_ID
            FROM CI_CLASS_TREE
            CONNECT BY PRIOR CLASS_ID = SUPER_CLASS_ID
            START WITH CLASS_ID = 30)
               AND (B.CLASS_ID = #{paramMap.classId} or B.INSTANCE_ID = #{paramMap.classId})
```

## 性能指标
性能指标从opentsdb中取数