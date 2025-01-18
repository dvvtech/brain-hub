TelemetryStorageReadDao с 41

Время выполнения этого метода около 100 мс.
Но когда этот метод вызывается очень часто из разных потоков то время его выполнения может достигать до 13 секунд.
В этом методе идет обращение к касандре. Как можно увеличить производительность?

```cs

List<DeviceMessageEntity> deviceMessageEntities = await _container.GetDeviceMessagesInRangeQuery.ExecuteAsync(deviceDbId, fromHour, toHour);

public async Task<List<DeviceMessageEntity>> ExecuteAsync(int deviceId, int fromHour, int toHour)
{
    CqlQuery<DeviceMessageEntity> deviceMessagesQuery = CreateCqlQuery(deviceId, fromHour, toHour);

    List<DeviceMessageEntity> deviceMessageEntities = (await deviceMessagesQuery.ExecuteAsync(_logger))
                                                                                .ToList();
    return deviceMessageEntities;
}

private CqlQuery<DeviceMessageEntity> CreateCqlQuery(int deviceId, int fromHour, int toHour)
{
    CqlQuery<DeviceMessageEntity> cqlQuery = _deviceMessagesTable.Where(x => x.DeviceId == deviceId &&
                                                                             x.Hour >= fromHour &&
                                                                             x.Hour <= toHour);

    return cqlQuery;
}

public static Task<IEnumerable<T>> ExecuteAsync<T>(this CqlQueryBase<T> query, ISpfLogger logger = null)
{
    if (query == null)
        return Task.FromResult(Enumerable.Empty<T>());

    return ExecuteDelegateAsync(query, query.ExecuteAsync, logger);
}

private static async Task<TU> ExecuteDelegateAsync<T, TU>(CqlQueryBase<T> query, Func<Task<TU>> executeAsync, ISpfLogger logger = null)
{
    if (logger == null)
    {
        return await executeAsync.Invoke();
    }            

    try
    {
        var result = await executeAsync.Invoke();                          
        return result;
    }
    catch (Exception e)
    {                
        object[] values;
        GetCqlDelegate<T> getCqlDelegate = GetDelegate<T>();
        string queryString = getCqlDelegate(query, out values);

        string valuesString = string.Join(",", values?.Select(x => x ?? "null") ?? Enumerable.Empty<object>());
        logger.LogError($"Запрос выполнился с ошибкой. {queryString};{valuesString};", e);
        throw;
    }
}
```

Увеличение производительности запросов к базе данных, такой как Cassandra, при частых вызовах из разных потоков может включать в себя несколько подходов и оптимизаций. Вот несколько стратегий, которые вы можете рассмотреть:

### 1. Оптимизация Запросов

- **Индексы**: Убедитесь, что ваши таблицы оптимизированы с точки зрения индексации для ускорения выборок.
- **Оптимизация запросов CQL**: Изучите запросы CQL и убедитесь, что они составлены эффективно для вашей структуры данных.
- **Пагинация**: Если возвращаемые наборы данных большие, рассмотрите возможность использования пагинации для уменьшения объема данных, передаваемых за один раз.

### 2. Кэширование Результатов

- **Кэширование на уровне приложения**: Рассмотрите возможность кэширования результатов запросов на уровне вашего приложения для сокращения количества обращений к базе данных.
- **Кэширование на уровне базы данных**: Используйте возможности кэширования, предоставляемые самой базой данных Cassandra.

### 3. Масштабирование и Конфигурация Cassandra

- **Кластеризация**: Убедитесь, что ваша база данных Cassandra правильно сконфигурирована и масштабирована. Это включает в себя настройку кластеров и узлов для распределения нагрузки.
- **Настройка производительности**: Оптимизируйте конфигурацию Cassandra для вашего конкретного случая использования (например, настройки памяти, производительности диска и т.д.).

### 4. Асинхронные и Параллельные Запросы

- **Асинхронная обработка**: Убедитесь, что запросы к Cassandra выполняются асинхронно, чтобы они не блокировали потоки.
- **Ограничение параллельности**: Если запросы выполняются из многих потоков одновременно, рассмотрите возможность введения механизма управления параллельностью, чтобы избежать перегрузки базы данных.

### 5. Мониторинг и Логирование

- **Мониторинг производительности**: Используйте инструменты для мониторинга производительности как вашего приложения, так и Cassandra.
- **Логирование и анализ**: Полезно вести подробное логирование, чтобы выявить узкие места в производительности.

### 6. Профилирование Приложения

- **Инструменты профилирования**: Используйте инструменты профилирования кода, чтобы выявить, где именно возникают задержки.

### 7. Оптимизация Сервера и Сети

- **Сетевая инфраструктура**: Убедитесь, что сетевая