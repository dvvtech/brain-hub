узнать степень фрагментации ключей

 ```sql
SELECT 
    dbschemas.name as SchemaName,
    dbtables.name as TableName,
    dbindexes.name as IndexName,
    indexstats.avg_fragmentation_in_percent,
    indexstats.page_count
FROM 
    sys.dm_db_index_physical_stats(DB_ID(), NULL, NULL, NULL, 'LIMITED') AS indexstats
    INNER JOIN sys.tables AS dbtables ON indexstats.object_id = dbtables.object_id
    INNER JOIN sys.schemas AS dbschemas ON dbtables.schema_id = dbschemas.schema_id
    INNER JOIN sys.indexes AS dbindexes ON indexstats.object_id = dbindexes.object_id
    AND indexstats.index_id = dbindexes.index_id
WHERE 
    indexstats.database_id = DB_ID()
ORDER BY 
    indexstats.avg_fragmentation_in_percent DESC;
```

```sql
--если фрагментация > 30%. Более ресурсоемкая операция.
--WITH (ONLINE = ON) чтоб таблица не блокировалась
ALTER INDEX [IndexName] ON [TableName] REBUILD WITH (ONLINE = ON); 
--если фрагментация < 30%. Менее ресурсоемкая операция
ALTER INDEX [IndexName] ON [TableName] REORGANIZE;

ALTER INDEX [IX_TerminalId] ON [BugReport].[dbo].[BugReports2] REBUILD
```

Запускаем приложение C:\DVV\GitProjects\tfs\ScoutSoftware\Development\BugReporting\BugReporting\Scout.BugReport.Console\App.config и указываем тс 200107 или 100500

Клиентское приложение при обращении к сервису иногда не дает ответа
и можно переписать его чтоб оно напрямую к бд обращалось.