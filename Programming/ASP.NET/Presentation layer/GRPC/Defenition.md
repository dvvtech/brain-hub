бинарный протокол
платформенно-независимый
быстрый

определяем прото файл
компилируем его
появляется автосгенеренный код
и потом в своем коде используете то что нагенерилось

типы операций:
unary - простой вызов
streaming (Client or Server) одиночные потоки с сервера или с клиента
bidirectional streaming и с сервера и с клинета идет поток

GRPC это протокол сериализации и протокол передачи данных от клинета к серверу

в протофайле

option csharp_namespace = "Demo.Grpc.Lib"      указываем какой неймспейс будет генерится

у Grpc есть интерцепторы. Урок 2_3. Интерцепторы относятся к конкретному grpc сервису
class LoggerInterceptor : Interceptor
{
	ctor(Ilogger logger)
}
Сначала отрабатывают Middleware, Interceptors и сам сервис

services.AddGrpcSwagger()
Воркшоп 2_3

services.AddGrpcClient<...>