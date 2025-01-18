```cs
public interface IUserRepository
{
    User GetById(int id);
    IEnumerable<User> GetAll();
    void Add(User user);
    void Update(User user);
    void Delete(int id);
}


```

```cs
public class UserRepository : IUserRepository
{
    private readonly DatabaseContext _dbContext;

    public UserRepository(DatabaseContext dbContext)
    {
        _dbContext = dbContext;
    }

    public User GetById(int id)
    {
        return _dbContext.Users.Find(id);
    }

    public IEnumerable<User> GetAll()
    {
        return _dbContext.Users.ToList();
    }

    public void Add(User user)
    {
        _dbContext.Users.Add(user);
        _dbContext.SaveChanges();
    }

    public void Update(User user)
    {
        _dbContext.Entry(user).State = EntityState.Modified;
        _dbContext.SaveChanges();
    }

    public void Delete(int id)
    {
        var user = _dbContext.Users.Find(id);
        if (user != null)
        {
            _dbContext.Users.Remove(user);
            _dbContext.SaveChanges();
        }
    }
}

```

Использование репозитория в бизнес логике

```cs
public class UserService
{
    private readonly IUserRepository _userRepository;

    public UserService(IUserRepository userRepository)
    {
        _userRepository = userRepository;
    }

    public void ChangeUserEmail(int userId, string newEmail)
    {
        var user = _userRepository.GetById(userId);
        if (user != null)
        {
            user.Email = newEmail;
            _userRepository.Update(user);
        }
    }

    // Другие методы работы с пользователями
}

```

```cs
public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<IUserRepository, UserRepository>();
    services.AddScoped<UserService>();
}

```