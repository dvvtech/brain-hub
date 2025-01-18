Open Closed Princip  принцип открытости закрытости
Программные сущности должны быть открыти для расширения и закрыты для изменения. 
Сделать это можно с помощью наследования

Нарушение принципа. Не отвалится старый функционал
```cs
class ReportGenerator
{
	void GeneratePdf() {...}
	void GenerateExcel() {...}
}
```

Правильный вариант

```cs
interface IReportGenerator
{
	void Generate();
}
class PdfReportGenerator : IReportGenerator
{
	void Generate() {...}
}
class ExcelReportGenerator : IReportGenerator
{
	void Generate() {...}
}
```