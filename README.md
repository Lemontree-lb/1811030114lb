
```sql
-- セッションの言語設定を us_english に設定
SET LANGUAGE us_english;

-- 変数の宣言
DECLARE @TableID int;

-- カーソルの宣言
DECLARE MSSQLCHK_CUR CURSOR FOR 
SELECT object_id FROM sys.objects WHERE  type IN('u','s');

-- カーソルのオープン
OPEN MSSQLCHK_CUR;

-- 最初の行のデータをカーソルに取得
FETCH MSSQLCHK_CUR INTO @TableID;

-- カーソルがデータを取得する間、繰り返す-- セッションの言語設定を us_english に設定
SET LANGUAGE us_english;

-- 変数の宣言
DECLARE @TableID int;

-- カーソルの宣言
DECLARE MSSQLCHK_CUR CURSOR FOR 
SELECT object_id FROM sys.objects WHERE  type IN('u','s');

-- カーソルのオープン
OPEN MSSQLCHK_CUR;

-- 最初の行のデータをカーソルに取得
FETCH MSSQLCHK_CUR INTO @TableID;

-- カーソルがデータを取得する間、繰り返す
WHILE( @@FETCH_STATUS = 0)
BEGIN
    -- sys.dm_db_index_physical_stats を使用して索引の断片化情報を取得
    SELECT
        OBJECT_NAME(IPS.OBJECT_ID) AS TableName,
        SI.NAME AS IndexName,
        IPS.AVG_FRAGMENTATION_IN_PERCENT,
        IPS.LEAF_PAGE_COUNT
    FROM
        SYS.DM_DB_INDEX_PHYSICAL_STATS(DB_ID(), NULL, NULL, NULL, 'DETAILED') IPS
    INNER JOIN
        SYS.INDEXES SI ON IPS.OBJECT_ID = SI.OBJECT_ID AND IPS.INDEX_ID = SI.INDEX_ID
    WHERE
        OBJECTPROPERTY(IPS.OBJECT_ID, 'IsUserTable') = 1
        AND OBJECT_NAME(IPS.OBJECT_ID) IN (SELECT name FROM sys.objects WHERE type IN ('U', 'S'))
        AND IPS.OBJECT_ID = @TableID;

    -- 次の行のデータをカーソルに取得
    FETCH MSSQLCHK_CUR INTO @TableID;
END;

-- カーソルのクローズ
CLOSE MSSQLCHK_CUR;

-- カーソルの解放
DEALLOCATE MSSQLCHK_CUR;

WHILE( @@FETCH_STATUS = 0)
BEGIN
    -- sys.dm_db_index_physical_stats を使用して索引の断片化情報を取得
    SELECT
        OBJECT_NAME(IPS.OBJECT_ID) AS TableName,
        SI.NAME AS IndexName,
        IPS.AVG_FRAGMENTATION_IN_PERCENT,
        IPS.LEAF_PAGE_COUNT
    FROM
        SYS.DM_DB_INDEX_PHYSICAL_STATS(DB_ID(), NULL, NULL, NULL, 'DETAILED') IPS
    INNER JOIN
        SYS.INDEXES SI ON IPS.OBJECT_ID = SI.OBJECT_ID AND IPS.INDEX_ID = SI.INDEX_ID
    WHERE
        OBJECTPROPERTY(IPS.OBJECT_ID, 'IsUserTable') = 1
        AND OBJECT_NAME(IPS.OBJECT_ID) IN (SELECT name FROM sys.objects WHERE type IN ('U', 'S'))
        AND IPS.OBJECT_ID = @TableID;

    -- 次の行のデータをカーソルに取得
    FETCH MSSQLCHK_CUR INTO @TableID;
END;

-- カーソルのクローズ
CLOSE MSSQLCHK_CUR;

-- カーソルの解放
DEALLOCATE MSSQLCHK_CUR;
-- 设置会话语言为 us_english
SET LANGUAGE us_english;

-- 声明变量
DECLARE @TableID int;

-- 声明游标
DECLARE MSSQLCHK_CUR CURSOR FOR 
SELECT object_id FROM sys.objects WHERE  type IN('u','s');

-- 打开游标
OPEN MSSQLCHK_CUR;

-- 获取游标的第一行数据
FETCH MSSQLCHK_CUR INTO @TableID;

-- 当游标获取数据时，执行以下操作
WHILE( @@FETCH_STATUS = 0)
BEGIN
    -- 使用 sys.dm_db_index_physical_stats 视图获取索引的平均碎片度和页数
    SELECT
        OBJECT_NAME(IPS.OBJECT_ID) AS TableName,
        SI.NAME AS IndexName,
        IPS.AVG_FRAGMENTATION_IN_PERCENT,
        IPS.LEAF_PAGE_COUNT
    FROM
        SYS.DM_DB_INDEX_PHYSICAL_STATS(DB_ID(), NULL, NULL, NULL, 'DETAILED') IPS
    INNER JOIN
        SYS.INDEXES SI ON IPS.OBJECT_ID = SI.OBJECT_ID AND IPS.INDEX_ID = SI.INDEX_ID
    WHERE
        OBJECTPROPERTY(IPS.OBJECT_ID, 'IsUserTable') = 1
        AND OBJECT_NAME(IPS.OBJECT_ID) IN (SELECT name FROM sys.objects WHERE type IN ('U', 'S'))
        AND IPS.OBJECT_ID = @TableID;

    -- 获取游标的下一行数据
    FETCH MSSQLCHK_CUR INTO @TableID;
END;

-- 关闭游标
CLOSE MSSQLCHK_CUR;

-- 释放游标资源
DEALLOCATE MSSQLCHK_CUR;

```

このコードは、`sys.dm_db_index_physical_stats` ビューを使用して索引の物理的な統計情報を取得し、`DBCC SHOWCONTIG` と同様の結果を得るためのものです。日本語での回答となります。

使用 sys.dm_db_index_physical_stats 来获取DBCC SHOWCONTIG查询结果中的Pages Scanned，Extents Scanned，Scan Density [Best Count: Actual Count]，Logical Scan FragmentationExtent Scan Fragmentation
