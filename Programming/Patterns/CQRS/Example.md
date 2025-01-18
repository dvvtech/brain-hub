```cs
// Команды
public interface ICommand
{
    void Execute();
}

// Запросы
public interface IQuery<T>
{
    T Execute();
}

public class CreateUserCommand : ICommand
{
    private readonly UserContext _context;
    private readonly string _userName;

    public CreateUserCommand(UserContext context, string userName)
    {
        _context = context;
        _userName = userName;
    }

    public void Execute()
    {
        var user = new User { Name = _userName };
        _context.Users.Add(user);
        _context.SaveChanges();
    }
}

public class GetAllUsersQuery : IQuery<IEnumerable<User>>
{
    private readonly UserContext _context;

    public GetAllUsersQuery(UserContext context)
    {
        _context = context;
    }

    public IEnumerable<User> Execute()
    {
        return _context.Users.ToList();
    }
}

public class UserController
{
    private readonly UserContext _context;

    public UserController(UserContext context)
    {
        _context = context;
    }

    public void CreateUser(string userName)
    {
        var command = new CreateUserCommand(_context, userName);
        command.Execute();
    }

    public IEnumerable<User> GetAllUsers()
    {
        var query = new GetAllUsersQuery(_context);
        return query.Execute();
    }
}

public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class UserContext : DbContext
{
    public DbSet<User> Users { get; set; }

    // Конфигурация контекста (например, строка подключения к БД)
}

```