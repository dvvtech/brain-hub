
```cs
//Доменная модель  (rich model)
class News
{
	public const MAX_TITLE_LENGTH = 100;

	public int Id {get;}
	public string Title {get;} = string.Empty;

	private News(int id, string title)
	{
		Id = id;
		Title = title;
	}

	public static Result<News> Create(int id, string title)
	{
		if(string.IsNullOrEmpty(title) || title.Lenght > MAX_TITLE_LENGTH)
		{
			Result.Failure<News>("title cannot be null or empty");
		}
		return Result.Success(new News(id, title));
	}
}
```