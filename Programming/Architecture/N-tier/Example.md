Разделение на слои с определенными ролями. Раньше был спагети код и все слои были перемешаны в одном месте.
В реальных проектах могут быть дополнительные слои, такие как слой безопасности, слой интеграции и т.д.

В этой архитектуре сложно написать модульные тесты для бизнес логики которые не связаны с БД - это сложно сделать.
```cs
// Presentation Layer (например, в ASP.NET MVC Controller)
public class UserController : Controller
{
    private UserService _userService;

    public UserController()
    {
        _userService = new UserService();
    }

    public ActionResult Index()
    {
        var users = _userService.GetAllUsers();
        return View(users);
    }
}

// Business Logic Layer
public class UserService
{
    private UserRepository _userRepository;

    public UserService()
    {
        _userRepository = new UserRepository();
    }

    public List<User> GetAllUsers()
    {
        return _userRepository.GetAll();
    }
}

// Data Access Layer
public class UserRepository
{
    public List<User> GetAll()
    {
        using (var context = new DatabaseContext())
        {
            return context.Users.ToList();
        }
    }
}

```


