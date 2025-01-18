Спецификация решает многократного использования сложных критериев фильтрации  в различных частях приложения.

```cs
public interface ISpecification<T>
{
    bool IsSatisfiedBy(T entity);
}

public class HighValueOrderSpecification : ISpecification<Order>
{
    private readonly decimal _minValue;

    public HighValueOrderSpecification(decimal minValue)
    {
        _minValue = minValue;
    }

    public bool IsSatisfiedBy(Order order)
    {
        return order.TotalValue >= _minValue;
    }
}

public class CustomerTypeSpecification : ISpecification<Order>
{
    private readonly CustomerType _customerType;

    public CustomerTypeSpecification(CustomerType customerType)
    {
        _customerType = customerType;
    }

    public bool IsSatisfiedBy(Order order)
    {
        return order.Customer.Type == _customerType;
    }
}

//Использование спецификаций

public class OrderService
{
    public IEnumerable<Order> FilterOrders(IEnumerable<Order> orders, ISpecification<Order> specification)
    {
        return orders.Where(order => specification.IsSatisfiedBy(order)).ToList();
    }
}

// Пример использования
var highValueSpec = new HighValueOrderSpecification(1000);
var customerTypeSpec = new CustomerTypeSpecification(CustomerType.VIP);

var filteredOrders = orderService.FilterOrders(allOrders, highValueSpec);
var vipOrders = orderService.FilterOrders(allOrders, customerTypeSpec);

```