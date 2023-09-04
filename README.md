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

