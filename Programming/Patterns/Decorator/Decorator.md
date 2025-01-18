Декораторы используются, когда нужно добавить новую функциональность или поведение к объекту, не изменяя его код.
Примеры:
* логирование
```cs
public class LoggingDecorator : IService
{
    private IService _service;

    public LoggingDecorator(IService service)
    {
        _service = service;
    }

    public void Execute()
    {
        Log("Executing service");
        _service.Execute();
        Log("Service executed");
    }

    private void Log(string message)
    {
        Console.WriteLine(message);
    }
}
```
- **Кэширование**: Добавление кэширования к методам, например, для ускорения запросов к базе данных.
    
- **Проверка безопасности**: Обертывание класса декоратором, который проверяет безопасность или права доступа перед выполнением основного метода.
    
- **Валидация**: Добавление валидации входных данных для метода.