**Область действия**: `Mutex` может использоваться для синхронизации потоков как внутри одного приложения, так и между разными процессами.
**Производительность**: `Mutex` обычно медленнее, чем `lock`, поскольку связан с большими накладными расходами на управление ресурсами.
```cs
using System;
using System.Threading;

class Program
{
    private static Mutex mutex = new Mutex();

    static void Main(string[] args)
    {
        for (int i = 0; i < 5; i++)
        {
            Thread newThread = new Thread(UseResource);
            newThread.Name = $"Поток {i}";
            newThread.Start();
        }

        Console.ReadLine();
    }

    private static void UseResource()
    {
        // Ожидание получения мьютекса
        Console.WriteLine($"{Thread.CurrentThread.Name} ожидает мьютекс");
        mutex.WaitOne();

        // Критическая секция
        Console.WriteLine($"{Thread.CurrentThread.Name} получил мьютекс");
        Thread.Sleep(1000); // Имитация работы

        // Освобождение мьютекса
        Console.WriteLine($"{Thread.CurrentThread.Name} освобождает мьютекс");
        mutex.ReleaseMutex();
    }
}

```