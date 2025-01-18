принцип говорит, что объекты в программе должны быть заменяемыми на экземпляры их подтипов без изменения правильности работы программы.

Нарушение принципа

```csharp
public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }

    public int CalculateArea()
    {
        return Width * Height;
    }
}

public class Square : Rectangle
{
    public override int Width
    {
        set { base.Width = base.Height = value; }
    }

    public override int Height
    {
        set { base.Width = base.Height = value; }
    }
}

void CalcSquare(Rectangle r)
{
	r.Width = 5;
	r.Height = 2;
	if(r.CalculateArea() != 10)
		throw new Exception("Не правильно посчитана площадь");
}
```

Исправление нарушения

```csharp
public interface IShape
{
    int CalculateArea();
}

public class Rectangle : IShape
{
    public int Width { get; set; }
    public int Height { get; set; }

    public int CalculateArea()
    {
        return Width * Height;
    }
}

public class Square : IShape
{
    public int SideLength { get; set; }

    public int CalculateArea()
    {
        return SideLength * SideLength;
    }
}
```