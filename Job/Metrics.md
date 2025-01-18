приложение сбора метрик
https://gitlab.scout-gps.ru:1444/platform/core-cloud/-/blob/web_dev/Main/Platform/Metrics/Scout.Platform.Metrics.Zookeeper/Program.cs

В gitlab в ветке statistics ввести в поиск metrix
wialon_migration_refact  в этой ветку добавлен Scout.Platform.Metrics.Zookeeper v1.0.5 для сбора метрик с платформы
https://gitlab.com/scout-cloud/core/-/commit/e25ef86a0638a8711c3437f2d296a9e4b3004e8e

Добавить Метрики. 2 экземпляра сервиса УСС. Метрики на каждый экземпляр. Если 2 экземпляра то 2 графика и на каждом графике линия это отдельный клиент.
Добавить пока кол-во запросов в минуту.

разобраться как разворачивать второй экземпляр УСС
и реализовать distibuted cach
