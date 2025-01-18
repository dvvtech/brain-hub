```cs
public abstract class InventoryCommand
{
	private readonly bool _isTerminatingCommand;
	internal InventoryCommand(bool commandIsTerminating)
	{
		_isTerminatingCommand = commandIsTerminating;
	}
	public bool RunCommand(out bool shouldQuit)
	{
		shouldQuit = _isTerminatingCommand;
		return InternalCommand();
	}	
	internal abstract bool InternalCommand();
	/* Еще вариан ф-ции
	public (bool wasSuccessful, bool shouldQuit) RunCommand()
	{
		...
		return (InternalCommand(), _isTerminatingCommand);
	}*/
}

public class HelpCommand : InventoryCommand
{
	public HelpCommand() : base(false) { }
	internal override bool InternalCommand()
	{
		Console.WriteLine("USAGE:");
		Console.WriteLine("\taddinventory (a)");
		...
		Console.WriteLine("Examples:");
		...
		return true;
	}
}
```

Рефактринг:

```cs
internal abstract class NonTerminatingCommand : InventoryCommand
{
	protected NonTerminatingCommand() : base(commandIsTerminating: false)
	{
	}
}

internal class HelpCommand : NonTerminatingCommand
{
	internal override bool InternalCommand()
	{
		Interface.WriteMessage("USAGE:");
		/* остальной код скрыт */
		return true;
	}
}

```