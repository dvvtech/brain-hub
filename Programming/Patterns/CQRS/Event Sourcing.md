
Event Sourcing — это паттерн архитектуры программного обеспечения, при котором изменения состояния приложения сохраняются как последовательность событий. Вместо того чтобы хранить текущее состояние объектов, система сохраняет список событий, произошедших с этими объектами. Эти события могут быть затем "проиграны" для воссоздания текущего состояния объекта в любой момент времени.

### Пример Event Sourcing

Рассмотрим систему управления заказами в интернет-магазине:

1. **Определение Событий**:
    
    - `OrderCreated`: заказ создан.
    - `ProductAddedToOrder`: продукт добавлен в заказ.
    - `ProductRemovedFromOrder`: продукт удалён из заказа.
    - `OrderShipped`: заказ отправлен.
    - `OrderCancelled`: заказ отменен.
2. **Хранение Событий**:
    
    - Вместо сохранения текущего состояния заказа, система сохраняет каждое из этих событий как отдельную запись.
3. **Восстановление Состояния**:
    
    - Чтобы определить текущее состояние заказа, система "проигрывает" все события по этому заказу. Например, если заказ был создан, затем в него был добавлен продукт, а потом заказ был отменен, последовательное применение этих событий покажет, что текущее состояние заказа — "отменен".

### В каких случаях использовать Event Sourcing

1. **Аудит и Отслеживание Изменений**:
    
    - Если требуется подробный аудит всех изменений в системе, Event Sourcing позволяет отслеживать каждое изменение состояния.
2. **Отказоустойчивость**:
    
    - Системы, где важна отказоустойчивость, могут использовать Event Sourcing для восстановления состояния после сбоев.
3. **Сложные Доменные Модели**:
    
    - В системах с сложной бизнес-логикой, где могут потребоваться различные представления одних и тех же данных, Event Sourcing позволяет легко реализовать и изменять эти представления.
4. **CQRS (Command Query Responsibility Segregation)**:
    
    - Event Sourcing естественно сочетается с CQRS, где операции чтения и записи разделены. События могут быть использованы для обновления различных моделей чтения.
5. **Масштабируемость и Распределенные Системы**:
    
    - В распределенных системах Event Sourcing может помочь в упрощении синхронизации состояния между различными узлами.

### Недостатки

- **Сложность**: Event Sourcing может добавить сложности в систему, особенно при внедрении в существующие приложения.
- **Производительность**: Построение текущего состояния путем "проигрывания" всех событий может быть ресурсоемким, особенно для объектов с большой историей изменений.
- **Миграция Событий**: Если бизнес-логика меняется, может потребоваться миграция уже сохраненных событий.

В целом, Event Sourcing — это мощный паттерн, но его внедрение следует тщательно обдумывать в контексте конкретных требований и целей приложения.

```cs
public interface IOrderEvent
{
    void Apply(Order order);
}

public class OrderCreated : IOrderEvent
{
    public DateTime CreationDate { get; set; }

    public void Apply(Order order)
    {
        order.CreationDate = CreationDate;
    }
}

public class ProductAddedToOrder : IOrderEvent
{
    public string ProductName { get; set; }

    public void Apply(Order order)
    {
        order.Products.Add(ProductName);
    }
}

public class OrderShipped : IOrderEvent
{
    public DateTime ShipDate { get; set; }

    public void Apply(Order order)
    {
        order.IsShipped = true;
        order.ShipDate = ShipDate;
    }
}


public class Order
{
    public DateTime CreationDate { get; set; }
    public bool IsShipped { get; set; }
    public DateTime? ShipDate { get; set; }
    public List<string> Products { get; set; } = new List<string>();

    public void ApplyEvent(IOrderEvent orderEvent)
    {
        orderEvent.Apply(this);
    }
}

public class OrderHistory
{
    private readonly List<IOrderEvent> _events;

    public OrderHistory(List<IOrderEvent> events)
    {
        _events = events;
    }

    public Order ReconstructOrder()
    {
        var order = new Order();
        foreach (var orderEvent in _events)
        {
            order.ApplyEvent(orderEvent);
        }
        return order;
    }
}


var events = new List<IOrderEvent>
{
    new OrderCreated { CreationDate = DateTime.UtcNow },
    new ProductAddedToOrder { ProductName = "Product 1" },
    new ProductAddedToOrder { ProductName = "Product 2" },
    new OrderShipped { ShipDate = DateTime.UtcNow.AddDays(1) }
};

var orderHistory = new OrderHistory(events);
var currentOrderState = orderHistory.ReconstructOrder();

Console.WriteLine($"Order Created on: {currentOrderState.CreationDate}");
Console.WriteLine($"Order Shipped: {currentOrderState.IsShipped}");
Console.WriteLine($"Order Products: {string.Join(", ", currentOrderState.Products)}");

```