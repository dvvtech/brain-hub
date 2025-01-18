Сначала определим репозитории для работы с заказами и товарами на складе.

```cs
public interface IOrderRepository
{
    void Add(Order order);
    // Другие методы для работы с заказами
}

public interface IStockRepository
{
    void UpdateStock(Item item, int quantity);
    // Другие методы для работы со складом
}

public class OrderRepository : IOrderRepository
{
    private readonly DatabaseContext _dbContext;

    public OrderRepository(DatabaseContext dbContext)
    {
        _dbContext = dbContext;
    }

    public void Add(Order order)
    {
        _dbContext.Orders.Add(order);
    }

    // Реализация других методов
}

public class StockRepository : IStockRepository
{
    private readonly DatabaseContext _dbContext;

    public StockRepository(DatabaseContext dbContext)
    {
        _dbContext = dbContext;
    }

    public void UpdateStock(Item item, int quantity)
    {
        // Логика обновления запасов на складе
    }

    // Реализация других методов
}

```

#### Использование Репозиториев и Unit of Work в Сервисе

```cs
public class OrderService
{
    private readonly IUnitOfWork _unitOfWork;
    private readonly IOrderRepository _orderRepository;
    private readonly IStockRepository _stockRepository;

    public OrderService(IUnitOfWork unitOfWork, IOrderRepository orderRepository, IStockRepository stockRepository)
    {
        _unitOfWork = unitOfWork;
        _orderRepository = orderRepository;
        _stockRepository = stockRepository;
    }

    public void CreateOrder(Order order, IDictionary<Item, int> items)
    {
        try
        {
            _orderRepository.Add(order);

            foreach (var item in items)
            {
                _stockRepository.UpdateStock(item.Key, item.Value);
            }

            _unitOfWork.Commit();
        }
        catch (Exception ex)
        {
            _unitOfWork.Rollback();
            throw;
        }
    }
}

```