События и делегаты в C# тесно связаны, но выполняют разные функции и имеют разные цели использования.
### Делегаты:

- **Делегат** — это тип, представляющий одну или несколько ссылок на методы с определённой сигнатурой, которая включает в себя список параметров и возвращаемый тип. Эту  сигнатуру и определяет делегат. Делегат можно передать в метод как аргумент, присвоить их переменным и возвращать их из других методов, используя тип делегата.
- Делегаты могут указывать на статические или экземплярные методы.

```csharp
public delegate int OperationDelegate(int x, int y);

public class Operations
{
    public static int Add(int x, int y)
    {
        return x + y;
    }

    public static int Multiply(int x, int y)
    {
        return x * y;
    }
}

		OperationDelegate addOperation = Operations.Add;

        // Вызов метода через делегат
        int addResult = addOperation(5, 10);
        Console.WriteLine($"Результат сложения: {addResult}");

        // Связывание делегата с другим методом
        addOperation = Operations.Multiply;

        // Повторный вызов метода через делегат
        int multiplyResult = addOperation(5, 10);

        OperationDelegate multyOperation = Operations.Add;
        multyOperation += Operations.Multiply;
    //Важно отметить, что только возвращаемое значение последнего метода в списке будет возвращено делегатом.
    var res = multyOperation(3, 4);//12
//Если нужно знать результат после каждого вызова метода как решение можно добавить в метод и делегат параметр List<int> results и в него сохранять результат
```

Примеры использования делегатов:

1. Callback методы (методы обратного вызова)

```csharp
public delegate void CallbackDelegate(string message);

public void Process(CallbackDelegate callback)
{
    // Обработка данных...
    callback("Обработка завершена");
}

public void MyCallbackMethod(string message)
{
    Console.WriteLine(message);
}

// Использование
Process(MyCallbackMethod);
```

внутри в качестве делегата  может быть метод для сортировки 2 значений
```csharp
ublic delegate int Comparison<T>(T x, T y);

public void Sort<T>(T[] items, Comparison<T> comparison)
{
    // Логика сортировки, использующая comparison для сравнения элементов
}

int CompareIntegers(int x, int y)
{
    return x - y;
}

// Использование
Sort(arrayOfIntegers, CompareIntegers);
```

2. Использование в событиях

```csharp
public event EventHandler MyEvent;

private void OnMyEvent()
{
    MyEvent?.Invoke(this, EventArgs.Empty);
}
```

3. Используется в LINQ и лямбда выражениях
Делегаты используются в LINQ для передачи логики фильтрации, сортировки и преобразования. Лямбда-выражения, которые являются сокращенной формой записи делегатов, широко применяются в LINQ.

```csharp
var filteredData = myData.Where(item => item.Value > 10);
```

Делегаты в C# могут указывать не только на синхронные методы, но и на асинхронные. Это позволяет использовать делегаты для вызова асинхронных методов, что особенно полезно в сценариях, когда необходимо работать с асинхронными операциями, такими как ввод-вывод, доступ к сети или базам данных, в контексте событий, обратных вызовов и т.д.

```csharp
using System;
using System.Threading.Tasks;

class Program
{
    // Определение делегата, который может указывать на асинхронный метод
    public delegate Task AsyncDelegate(string message);

    static async Task Main(string[] args)
    {
        // Создание экземпляра делегата, указывающего на асинхронный метод
        AsyncDelegate del = AsyncMethod;

        // Вызов асинхронного метода через делегат с использованием await
        await del("Hello, World!");

        Console.WriteLine("Done");
    }

    // Асинхронный метод, который будет вызван через делегат
    public static async Task AsyncMethod(string message)
    {
        await Task.Delay(1000); // Имитация асинхронной операции
        Console.WriteLine(message);
    }
}
```