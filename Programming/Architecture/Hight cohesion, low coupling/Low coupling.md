Low Coupling (Низкая Связанность) — это принцип проектирования в разработке программного обеспечения, который стремится уменьшить зависимости между различными частями программы. Это облегчает поддержку, тестирование и расширение программного обеспечения. Увеличивает переиспользование компонентов.

### Пример низкой связности:

Допустим, у вас есть веб-приложение для управления заказами, и есть два основных компонента: один для управления клиентскими данными, а другой для обработки заказов.

#### Без применения low coupling:
```cs
public class CustomerService
{
    public Customer GetCustomer(int id) 
    {
        // Получение данных клиента
    }

    // Другие методы, связанные с клиентами...
}

public class OrderService
{
    private CustomerService _customerService;

    public OrderService()
    {
        _customerService = new CustomerService();
    }

    public void ProcessOrder(Order order)
    {
        Customer customer = _customerService.GetCustomer(order.CustomerId);
        // Обработка заказа
    }

    // Другие методы, связанные с заказами...
}

```

В этом примере `OrderService` прямо зависит от `CustomerService`, создавая высокую связность. Изменения в `CustomerService` могут потребовать изменений в `OrderService`.

#### С применением low coupling:
```cs
public interface ICustomerService
{
    Customer GetCustomer(int id);
}

public class CustomerService : ICustomerService
{
    public Customer GetCustomer(int id) 
    {
        // Получение данных клиента
    }
    // Другие методы, связанные с клиентами...
}

public class OrderService
{
    private ICustomerService _customerService;

    public OrderService(ICustomerService customerService)
    {
        _customerService = customerService;
    }

    public void ProcessOrder(Order order)
    {
        Customer customer = _customerService.GetCustomer(order.CustomerId);
        // Обработка заказа
    }

    // Другие методы, связанные с заказами...
}

```
В этом примере `OrderService` зависит от абстракции `ICustomerService`, а не от конкретной реализации `CustomerService`. Это уменьшает связность, так как `OrderService` теперь менее зависим от внутренней реализации работы с клиентами и может быть легко протестирован или изменен без необходимости изменения в `CustomerService`. 

Мое: В нашем компоненте может быть несколько классов и когда в компоненте объявлен интерфейс это может говорить что взаимодействие с ним нужно вести через этот интерфейс.
Внутри компонента тоже могут быть интерфейсы. И чтобы заиспользовать компонент в другой части программы нужно изменить зависимость.

### Преимущества Low Coupling:

- Упрощает тестирование (каждый компонент может быть протестирован независимо).
- Повышает переиспользуемость компонентов.
- Упрощает понимание и поддержку кода.

А если сильная связанность - то когда хочешь внести изменения в одном месте и тебе приходится внести изменения еще в 6 местах

Сильная связанность нарушает Принцип Открытости/Закрытости