Допустим, у нас есть сервис обработки заказов, который после создания заказа должен отправить событие другим сервисам.

```cs
public class OutboxMessage
{
    public int Id { get; set; }
    public string EventType { get; set; }
    public string Data { get; set; }
    public bool IsSent { get; set; } // Поле, указывающее, было ли сообщение отправлено
    // Остальные свойства и конструктор
}

public class OrderService
{
    private readonly DatabaseContext _dbContext;

    public OrderService(DatabaseContext dbContext)
    {
        _dbContext = dbContext;
    }

    public void CreateOrder(Order order)
    {
        // Сохранение заказа в базе данных
        _dbContext.Orders.Add(order);

        // Создание сообщения для Outbox
        var outboxMessage = new OutboxMessage("OrderCreated", Serialize(order));
        _dbContext.OutboxMessages.Add(outboxMessage);

        // Фиксация изменений в базе данных
        _dbContext.SaveChanges();
    }

    private string Serialize(object obj)
    {
        // Сериализация объекта в JSON или другой формат
        return JsonConvert.SerializeObject(obj);
    }
}

```


```cs
public class OutboxPublisher : IHostedService
{
    private readonly DatabaseContext _dbContext;
    
    public OutboxPublisher(DatabaseContext dbContext)
    {
        _dbContext = dbContext;
    }

    // Запуск фонового сервиса
    public Task StartAsync(CancellationToken cancellationToken)
    {
        // Запуск фоновой задачи для отправки сообщений из outbox
        ProcessOutboxMessages(cancellationToken);
        return Task.CompletedTask;
    }

    private void ProcessOutboxMessages(CancellationToken cancellationToken)
    {
        while (!cancellationToken.IsCancellationRequested)
        {
            var messagesToSend = _dbContext.OutboxMessages.Where(m => !m.IsSent).ToList();

            foreach (var message in messagesToSend)
            {
                // Логика отправки сообщения в очередь или другой сервис
                // После успешной отправки, помечаем сообщение как отправленное
                message.IsSent = true;
                _dbContext.SaveChanges();
            }

            // Частота проверки или отправки можно настроить
            Thread.Sleep(1000); // Пауза 1 секунда, например
        }
    }

    // Остановка фонового сервиса
    public Task StopAsync(CancellationToken cancellationToken)
    {
        // Логика для остановки сервиса, если необходимо
        return Task.CompletedTask;
    }
}

```

```cs
public void ConfigureServices(IServiceCollection services)
{
    // ... другие сервисы

    services.AddHostedService<OutboxPublisher>();
}

```