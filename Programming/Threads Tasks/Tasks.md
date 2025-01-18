1. Non awaited task hide exceptions
2. Так писать нормально
```cs
public async Task<ActionResult<DriverModel>> GetAvailableDriverAsync(int id)
{
	try
	{
		var unit = await GetUnitById(unitId, sessionId);
	}
	catch(Exception e)
	{	
	}
}

public Task<UnitDto> GetUnitById(int id, string scoutAuthorization)
{
    var path = ScoutHttpUriHelper.CreateReadOnlyMethodUri("get");
    return _proxy.PostAsync<ObjectIdRequest, UnitDto>(id, path, scoutAuthorization);
}
```