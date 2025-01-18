1. Для плагина эковождение нужно отдельно собирать сборку для беты и прода
так как не выносил адрес сервиса в конфиг
это нужно сделать в конструкторе класса EcoDrivingService
 _httpClient.BaseAddress = new Uri("https://beco-driving.scout-gps.ru:9025/profiles/");//для беты
 httpClient.BaseAddress = new Uri("https://eco-driving.scout-gps.ru:9025/profiles/");//для прода