public class TcpCommandsController
{
    public static TcpCommandsController Instance => InstanceFactory.Value;

    private static readonly Lazy<TcpCommandsController> InstanceFactory =
        new Lazy<TcpCommandsController>(() => new TcpCommandsController());    

    private readonly HashSet<Guid> _availableDeviceTypes;

    private TcpCommandsController()
    {
        _availableDeviceTypes = new HashSet<Guid>() { ScoutDevices.ScoutMt850Plus };
    }

    public bool CheckDevice(Guid deviceType)
    {
        return _availableDeviceTypes.Contains(deviceType);
    }
}