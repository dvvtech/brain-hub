![[Pasted image 20240316140500.png]]
```cs
record Person(string Name, int Age)
{
    public string FullName() => $"{Name} {Age}";
}

Person person = new Person("dvv", 30);
var person2 = person with { Age = 32 };
```