### Описание:

- В этом примере мы определили два разных координатора саг: `OrderProcessingSagaCoordinator` и `ReturnManagementSagaCoordinator`, каждый из которых управляет разными бизнес-процессами.
- `OrderProcessingSagaCoordinator` координирует процесс создания заказа, обработки платежа и доставки.
- `ReturnManagementSagaCoordinator` координирует процесс возврата товара и возврата средств.
- Каждый координатор саги отвечает за последовательное выполнение операций в рамках своей саги и обработку возможных ошибок.

Этот пример демонстрирует, как в рамках одной системы могут быть реализованы несколько саг для управления различными бизнес-процессами. Важно отметить, что в реальных приложениях эти саги могут быть гораздо более сложными и включать асинхронную коммуникацию между микросервисами, компенсационные транзакции и дополнительную логику управления состоянием.

```cs
// Общие классы и интерфейсы
public abstract class SagaEvent {}
public class SagaFailedEvent : SagaEvent {}

// События для Саги Обработки Заказа
public class OrderCreatedEvent : SagaEvent {}
public class PaymentProcessedEvent : SagaEvent {}
public class DeliveryArrangedEvent : SagaEvent {}

// События для Саги Управления Возвратами
public class ReturnRequestedEvent : SagaEvent {}
public class ReturnProcessedEvent : SagaEvent {}
public class RefundProcessedEvent : SagaEvent {}

// Представление сервисов
public class OrderService { /* Методы для обработки заказа */ }
public class PaymentService { /* Методы для обработки платежа */ }
public class DeliveryService { /* Методы для организации доставки */ }

public class ReturnService { /* Методы для обработки возврата */ }
public class RefundService { /* Методы для обработки возврата средств */ }

// Координаторы Саг
public class OrderProcessingSagaCoordinator
{
    private OrderService orderService = new OrderService();
    private PaymentService paymentService = new PaymentService();
    private DeliveryService deliveryService = new DeliveryService();

    public void HandleOrderProcessingSaga()
    {
        // Логика обработки саги обработки заказа
    }
}

public class ReturnManagementSagaCoordinator
{
    private ReturnService returnService = new ReturnService();
    private RefundService refundService = new RefundService();

    public void HandleReturnManagementSaga()
    {
        // Логика обработки саги управления возвратами
    }
}

// Использование
public class Program
{
    public static void Main()
    {
        var orderProcessingCoordinator = new OrderProcessingSagaCoordinator();
        orderProcessingCoordinator.HandleOrderProcessingSaga();

        var returnManagementCoordinator = new ReturnManagementSagaCoordinator();
        returnManagementCoordinator.HandleReturnManagementSaga();
    }
}
```