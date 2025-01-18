[[Factory Method]] тут пример богатой модели
### Бедная модель (Anemic Model)

Бедная модель характеризуется тем, что она содержит мало или совсем не содержит бизнес-логики. Такие модели обычно представляют собой простые объекты для передачи данных (Data Transfer Objects, DTOs), содержащие только свойства и минимальный или отсутствующий методы.

```csharp
public class User
{
    public string Name { get; set; }
    public string Email { get; set; }
    public DateTime DateOfBirth { get; set; }
    // Другие свойства
}

public class UserService
{
    public void RegisterUser(User user)
    {
        if (string.IsNullOrWhiteSpace(user.Name))
        {
            throw new ArgumentException("Name is required");
        }

        if (string.IsNullOrWhiteSpace(user.Email))
        {
            throw new ArgumentException("Email is required");
        }

        // Другие проверки и логика регистрации
    }
}
```

В этом примере класс `User` просто хранит данные, а вся бизнес-логика сконцентрирована в классе `UserService`.

### Богатая модель (Rich Domain Model)

В богатой модели бизнес-логика инкапсулирована внутри классов модели. Модели не только хранят данные, но и содержат поведение и правила, которые связаны с этими данными.

```csharp
public class User
{
    public string Name { get; private set; }
    public string Email { get; private set; }
    public DateTime DateOfBirth { get; private set; }

    public User(string name, string email, DateTime dateOfBirth)
    {
        if (string.IsNullOrWhiteSpace(name))
        {
            throw new ArgumentException("Name is required");
        }

        if (string.IsNullOrWhiteSpace(email))
        {
            throw new ArgumentException("Email is required");
        }

        Name = name;
        Email = email;
        DateOfBirth = dateOfBirth;
    }

    public void UpdateEmail(string newEmail)
    {
        if (string.IsNullOrWhiteSpace(newEmail))
        {
            throw new ArgumentException("Email cannot be empty.");
        }

        Email = newEmail;

        // Дополнительная логика, связанная с обновлением email
    }

    // Другие методы, связанные с бизнес-логикой
}
```


В этом примере класс `User` сам управляет своим состоянием и содержит логику, связанную с данными пользователя. Это делает класс `User` более автономным и лучше отражает бизнес-правила, связанные с понятием пользователя.

### Вывод

Выбор между бедной и богатой моделями зависит от конкретных требований к приложению и предпочтений команды. Бедные модели могут быть проще и более гибкими при интеграции различных слоев приложения, в то время как богатые модели лучше подходят для сложных предметных областей, где важна инкапсуляция и явное представление бизнес-логики.