`ReaderWriterLock` в C# предназначен для сценариев, когда у вас есть ресурс, к которому осуществляются как операции чтения, так и операции записи, причём операций чтения обычно значительно больше, чем операций записи. `ReaderWriterLock` позволяет множеству потоков безопасно читать данные одновременно, но требует эксклюзивного доступа для потока, который записывает данные.
```cs
using System;
using System.Threading;

class ReaderWriterExample
{
    private static ReaderWriterLock rwLock = new ReaderWriterLock();
    private static Random random = new Random();
    private static string sharedResource = "Начальные данные";

    static void Main()
    {
        Thread writerThread = new Thread(WriteResource);
        writerThread.Start();

        for (int i = 0; i < 5; i++)
        {
            Thread readerThread = new Thread(ReadResource);
            readerThread.Start();
        }
    }

    static void ReadResource()
    {
        try
        {
            rwLock.AcquireReaderLock(Timeout.Infinite);
            Console.WriteLine($"Читающий поток {Thread.CurrentThread.ManagedThreadId} читает '{sharedResource}'");
            // Имитация продолжительного чтения
            Thread.Sleep(random.Next(1000, 2000));
        }
        finally
        {
            rwLock.ReleaseReaderLock();
        }
    }

    static void WriteResource()
    {
        try
        {
            rwLock.AcquireWriterLock(Timeout.Infinite);
            sharedResource = $"Данные записаны потоком {Thread.CurrentThread.ManagedThreadId}";
            Console.WriteLine($"Пишущий поток {Thread.CurrentThread.ManagedThreadId} записывает '{sharedResource}'");
            // Имитация продолжительной записи
            Thread.Sleep(random.Next(1000, 2000));
        }
        finally
        {
            rwLock.ReleaseWriterLock();
        }
    }
}

```