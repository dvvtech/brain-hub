Нарушение. Обычно когда не верно спроектировали классы.

```csharp
public class Bird
{
    public virtual void Fly()
    {
        // Реализация полёта
    }
}

public class Penguin : Bird
{
    public override void Fly()
    {
        throw new NotImplementedException("Пингвины не летают");
    }
}
```

Исправление

```csharp
public interface IBird
{
    void Eat();
}

public interface IFlyingBird : IBird
{
    void Fly();
}

public class Bird : IFlyingBird
{
    public void Eat()
    {
        // Реализация поедания
    }

    public void Fly()
    {
        // Реализация полёта
    }
}

public class Penguin : IBird
{
    public void Eat()
    {
        // Реализация поедания
    }
}
```

Это обеспечивает соблюдение LSP, так как теперь все подтипы `IBird` могут использоваться взаимозаменяемо без нарушения ожидаемого поведения.