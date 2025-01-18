Подобен шлагбауму. Когда перевели в сигнальное состояние все потоки которые висят на WaitOne начинают ломиться до тех пор пока явно не переведем в несигнальное состояние.

Сначала идем до WaitOne и ожидаем Set. После Set разблокируется WaitOne. И даже если потом будет WaitOne то он блокироваться не будет. А чтобы он заблокировался нужно вызвать Reset.
После вызова метода `Set`, событие остаётся в сигнальном состоянии до тех пор, пока явно не будет вызван метод `Reset`.
```cs
using System;
using System.Threading;
using System.Threading.Tasks;

class Program
{
//инициализируется в несигнальном состоянии (`false`), что означает, что потоки, ожидающие на `manualResetEvent.WaitOne()`, будут блокироваться до получения сигнала.
    static ManualResetEvent manualResetEvent = new ManualResetEvent(false);

    static void Main(string[] args)
    {
        Console.WriteLine("Главный поток запущен.");

        Task.Run(() => DoWork());

        Console.WriteLine("Ждем завершения работы второстепенного потока.");
        manualResetEvent.WaitOne(); // Ожидаем сигнала

        Console.WriteLine("Второстепенный поток завершил работу.");
    }

    static void DoWork()
    {
        Console.WriteLine("Второстепенный поток начал работу.");
        Thread.Sleep(5000); // Имитация длительной работы
        Console.WriteLine("Второстепенный поток завершил работу.");

        manualResetEvent.Set(); // Отправляем сигнал основному потоку, что работа выполнена
    }
}

```