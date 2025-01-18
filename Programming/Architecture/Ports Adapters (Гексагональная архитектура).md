Бизнес-логика (Core)
```cs
// Интерфейс выходного порта
public interface IDataAccess
{
    void SaveData(string data);
}

// Класс бизнес-логики
public class DataProcessor
{
    private readonly IDataAccess _dataAccess;

    public DataProcessor(IDataAccess dataAccess)
    {
        _dataAccess = dataAccess;
    }

    public void ProcessData(string data)
    {
        // Процесс обработки данных
        _dataAccess.SaveData(data);
    }
}

```

Выходной адаптер (например, для доступа к базе данных)
```cs
// Реализация интерфейса IDataAccess
public class DatabaseDataAccess : IDataAccess
{
    public void SaveData(string data)
    {
        // Логика сохранения данных в базе данных
    }
}

```

Входной адаптер (например, REST API)
```cs
public class DataController : ApiController
{
    private readonly DataProcessor _dataProcessor;

    public DataController(DataProcessor dataProcessor)
    {
        _dataProcessor = dataProcessor;
    }

    [HttpPost]
    public void PostData(string data)
    {
        _dataProcessor.ProcessData(data);
    }
}

```

Конфигурация зависимостей
```cs
// В вашем стартовом классе или контейнере DI
var dataProcessor = new DataProcessor(new DatabaseDataAccess());
var dataController = new DataController(dataProcessor);

```


Пример с работы:

![[Pasted image 20231212210308.png]]

Из предметной области, выделены доменные сущности, которые позволяют провести границы модулей, за которые нельзя выходить – каждый модуль решает конкретную бизнес-задачу. Модуль не может напрямую общаться с другим модулем, а делает это через порты – наши границы, организующие связь с внешним миром. Эти границы представлены в коде интерфейсами, которые реализуют адаптеры. Таким образом мы получаем слабую связанность.