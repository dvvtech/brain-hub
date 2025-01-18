Если число параметров больше 3 - 4 то возможно их следует выделить в класс
Метод становится более читаемым

```cs
record ReportOptions(
	string Title,
	string Header,
	string Footer,
	DateTime Date,
	List<Data> ReportData,
	bool PrintReview,
	bool SaveToFile);

void GenerateReport(
	ReportOptions options
) {}
```