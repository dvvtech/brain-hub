[[3. Q3 Sql 1 or more Telephone]]
```csharp
using MongoDB.Bson;
using MongoDB.Driver;
using System;

namespace MongoDbExample
{
    class Program
    {
        static void Main(string[] args)
        {
            // Создание клиента для подключения к MongoDB
            var client = new MongoClient("mongodb://localhost:27017");

            // Получение базы данных
            var database = client.GetDatabase("yourDatabaseName");

            // Получение коллекции
            var collection = database.GetCollection<BsonDocument>("Person");

            // Создание нового документа
            var newPerson = new BsonDocument
            {
                { "Name", "Иван Иванов" },
                { "Age", 30 },
                { "Address", "ул. Пушкина, д.10" },
                { "Phones", new BsonArray { "+71234567890", "+70987654321" } }
            };

            // Сохранение документа в коллекцию
            collection.InsertOne(newPerson);
        
			// Фильтрация данных по имени
            var filter = Builders<BsonDocument>.Filter.Eq("Name", "Иван Иванов");
            var filteredPeople = collection.Find(filter).ToList();

            Console.WriteLine("Запись успешно добавлена в MongoDB");
        }
    }
}
```
