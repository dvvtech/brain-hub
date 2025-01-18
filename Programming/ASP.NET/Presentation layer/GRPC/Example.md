### Шаг 1: Регистрация gRPC-клиента

Сначала убедимся, что gRPC-клиент правильно зарегистрирован в `Startup.cs`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpcClient<MyGrpcService.GrpcServiceClient>(options =>
    {
        options.Address = new Uri("https://mygrpcserviceurl");
    });
}
```
### Шаг 2: Создание сервиса или контроллера

Теперь создадим сервис или контроллер, который будет использовать этот gRPC-клиент:

```csharp
public class MyService
{
    private readonly MyGrpcService.GrpcServiceClient _grpcClient;

    public MyService(MyGrpcService.GrpcServiceClient grpcClient)
    {
        _grpcClient = grpcClient;
    }

    public async Task<SomeResponseType> DoSomethingAsync()
    {
        // Создание запроса
        var request = new SomeRequestType();

        // Вызов метода на gRPC-сервисе
        SomeResponseType response = await _grpcClient.SomeRpcMethodAsync(request);

        return response;
    }
}
```

В этом примере `MyService` получает `MyGrpcService.GrpcServiceClient` через конструктор. Затем в методе `DoSomethingAsync` мы используем этот клиент для выполнения RPC-вызова (`SomeRpcMethodAsync`).