Минимизация зависимости от инфраструктуры является ключевой особеностью этой архитектуры. Используется для сосредоточения на доменной модели.

Абстракции помещаются в модель предметной области и уровень инфраструктуры реализует эти абстракции. И БЛ не зависит от инфраструктуры. Папка Domain, Infrastructure

```cs
// Entities (Core Domain)
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    // другие свойства пользователя
}

// Use Cases (Application Business Rules)
public class UserUseCases
{
    private readonly IUserRepository _userRepository;

    public UserUseCases(IUserRepository userRepository)
    {
        _userRepository = userRepository;
    }

    public List<User> GetAllUsers()
    {
        return _userRepository.GetAll();
    }

    // другие методы, связанные с пользователями
}

// Interface Adapters (Data Access Implementation)
public class UserRepository : IUserRepository
{
    public List<User> GetAll()
    {
        using (var context = new DatabaseContext())
        {
            return context.Users.ToList();
        }
    }
}

// Interface Adapters (Presentation)
public class UserController
{
    private readonly UserUseCases _userUseCases;

    public UserController(UserUseCases userUseCases)
    {
        _userUseCases = userUseCases;
    }

    public ActionResult Index()
    {
        var users = _userUseCases.GetAllUsers();
        return View(users);
    }
}

// Dependency Injection configuration (somewhere in your startup or main file)
var userUseCases = new UserUseCases(new UserRepository());
var userController = new UserController(userUseCases);

```