ReportStreamingManager забираем из кафки реквест на построение отчета


1. [thrd:main]: Application maximum poll interval (300000ms) exceeded by 187ms (adjust max.poll.interval.ms for long-running message processing): leaving group

Эта ошибка связана с Apache Kafka и возникает, когда потребитель (consumer) не успевает обработать сообщения в пределах установленного максимального интервала опроса (max.poll.interval.ms).

Разберем ошибку:

- Максимальный интервал опроса установлен на 300000 мс (5 минут).
- Этот интервал был превышен на 187 мс.
- В результате потребитель покидает группу.

Причины:

- Слишком долгая обработка сообщений.
- Большой объем данных в каждом пакете сообщений.
- Недостаточные ресурсы для быстрой обработки.

Варианты решения:
 оптимизации обработки сообщений

2. [upsertCommand] The operation has timed out Npgsql.NpgsqlException (0x80004005): The operation has timed out
 ---> System.TimeoutException: The operation has timed out.
   at Npgsql.ThrowHelper.ThrowNpgsqlExceptionWithInnerTimeoutException(String message)
   at Npgsql.Util.NpgsqlTimeout.Check()
   at Npgsql.Util.NpgsqlTimeout.CheckAndGetTimeLeft()
   at Npgsql.Util.NpgsqlTimeout.CheckAndApply(NpgsqlConnector connector)
   at Npgsql.Internal.NpgsqlConnector.<Open>g__OpenCore|213_1(NpgsqlConnector conn, SslMode sslMode, NpgsqlTimeout timeout, Boolean async, CancellationToken cancellationToken, Boolean isFirstAttempt)
   at Npgsql.Internal.NpgsqlConnector.Open(NpgsqlTimeout timeout, Boolean async, CancellationToken cancellationToken)
   at Npgsql.PoolingDataSource.OpenNewConnector(NpgsqlConnection conn, NpgsqlTimeout timeout, Boolean async, CancellationToken cancellationToken)
   at Npgsql.PoolingDataSource.<Get>g__RentAsync|34_0(NpgsqlConnection conn, NpgsqlTimeout timeout, Boolean async, CancellationToken cancellationToken)
   at Npgsql.NpgsqlConnection.<Open>g__OpenAsync|42_0(Boolean async, CancellationToken cancellationToken)
   at Npgsql.NpgsqlConnection.Open()
   at Scout.Platform.Storage.Reports.Postgres.ReportStoragePostgresProvider._Set(String upsertSql, Guid id, Byte[] binaryData, Int32 accountId) in /src/Platform/Storage/Reports/Scout.Platform.Storage.Reports/Postgres/ReportStoragePostgresProvider.cs:line 250
   
3. [08-02 07:47:25 ERR] [ad3a1afa-309c-41c6-9361-1425c7c78781] Ошибка записи результата построения отчета в Redis. [Unknown]null 
4. ReportingHubClient error: Response status code does not indicate success: 404 (Not Found).
5. ReportingHubClient error: The server disconnected before the handshake could be started.
6. [08-02 14:27:55 ERR] Status(StatusCode="Unavailable", Detail="Error starting gRPC call. HttpRequestException: The SSL connection could not be established, see inner exception. IOException: Received an unexpected EOF or 0 bytes from the transport stream.", DebugException="System.Net.Http.HttpRequestException: The SSL connection could not be established, see inner exception.") Grpc.Core.RpcException: Status(StatusCode="Unavailable", Detail="Error starting gRPC call. HttpRequestException: The SSL connection could not be established, see inner exception. IOException: Received an unexpected EOF or 0 bytes from the transport stream.", DebugException="System.Net.Http.HttpRequestException: The SSL connection could not be established, see inner exception.") ---> System.Net.Http.HttpRequestException: The SSL connection could not be established, see inner exception. ---> System.IO.IOException: Received an unexpected EOF or 0 bytes from the transport stream. at System.Net.Security.SslStream.ReceiveHandshakeFrameAsync[TIOAdapter](CancellationToken cancellationToken) at System.Net.Security.SslStream.ForceAuthenticationAsync[TIOAdapter](Boolean receiveFirst, Byte[] reAuthenticationData, CancellationToken cancellationToken) at System.Net.Http.ConnectHelper.EstablishSslConnectionAsync(SslClientAuthenticationOptions sslOptions, HttpRequestMessage request, Boolean async, Stream stream, CancellationToken cancellationToken) --- End of inner exception stack trace --- at System.Net.Http.ConnectHelper.EstablishSslConnectionAsync(SslClientAuthenticationOptions sslOptions, HttpRequestMessage request, Boolean async, Stream stream, CancellationToken cancellationToken) at System.Net.Http.HttpConnectionPool.ConnectAsync(HttpRequestMessage request, Boolean async, CancellationToken cancellationToken) at System.Net.Http.HttpConnectionPool.AddHttp2ConnectionAsync(QueueItem queueItem) at System.Threading.Tasks.TaskCompletionSourceWithCancellation`1.WaitWithCancellationAsync(CancellationToken cancellationToken) at System.Net.Http.HttpConnectionPool.SendWithVersionDetectionAndRetryAsync(HttpRequestMessage request, Boolean async, Boolean doRequestAuth, CancellationToken cancellationToken) at System.Net.Http.RedirectHandler.SendAsync(HttpRequestMessage request, Boolean async, CancellationToken cancellationToken) at Grpc.Net.Client.Balancer.Internal.BalancerHttpHandler.SendAsync(HttpRequestMessage request, CancellationToken cancellationToken) at Grpc.Net.Client.Internal.GrpcCall`2.RunCall(HttpRequestMessage request, Nullable`1 timeout) --- End of inner exception stack trace --- at Scout.Geofences.GrpcClient.FullGeofencesGrpcSearcher.FindGeofenceIdsListAsync(GeofenceSearchRequestList request) in /src/Platform/Components/Geofences/Scout.Geofences.GrpcClient/FullGeofencesGrpcSearcher.cs:line 24 at Scout.Geofences.GrpcClient.GeofencesGrpcSearcher.FindGeofenceIdsAsync(MultiLocationsGeofenceRequestInfo requestInfo) in /src/Platform/Components/Geofences/Scout.Geofences.GrpcClient/GeofencesGrpcSearcher.cs:line 98 at Scout.Platform.Streaming.Reports.RawStatistics.CoreStatistics.Sgv.GeofenceVisitsStatisticsBuilder.CreateGeozoneVisits(DateTimeRange period, IGeofenceSearcher searcher, HashSet`1 geofenceIdFilter, NavigationFiltrationStatistics navigationStatistic) in /src/Platform/Streaming/Reports/Scout.Platform.Streaming.Reports.RawStatistics/CoreStatistics/Sgv/GeofenceVisitsStatisticsBuilder.cs:line 301 at Scout.Platform.Streaming.Reports.RawStatistics.CoreStatistics.Sgv.GeofenceVisitsStatisticsBuilder.BuildStatisticsChunkAsync(StatisticsBuildChunkContext context, CancellationToken cancellationToken) in /src/Platform/Streaming/Reports/Scout.Platform.Streaming.Reports.RawStatistics/CoreStatistics/Sgv/GeofenceVisitsStatisticsBuilder.cs:line 159 at Scout.Platform.Streaming.Reports.RawStatistics.Processing.ChunksBuilder.BuildChunkAsync(StatisticsProcessingContext statisticsContext, CancellationToken cancellation) in /src/Platform/Streaming/Reports/Scout.Platform.Streaming.Reports.RawStatistics/Processing/ChunkBuilder.cs:line 334
7. Старт построения: account null

174

Period: 02.08.2024 08:00:00 - 03.08.2024 23:00:00, UnitIds:82632

175

Отчет: ДиСсТ, Кол-во машинодней: 2  

176

[08-03 04:16:21 ERR] ДиСсТ Geofences exception  

177

Grpc.Core.RpcException: Status(StatusCode="Unavailable", Detail="Error connecting to subchannel.", DebugException="System.Net.Sockets.SocketException: Connection refused")

178

 ---> System.Net.Sockets.SocketException (111): Connection refused

179

   at System.Net.Sockets.Socket.AwaitableSocketAsyncEventArgs.ThrowException(SocketError error, CancellationToken cancellationToken)

180

   at System.Net.Sockets.Socket.AwaitableSocketAsyncEventArgs.System.Threading.Tasks.Sources.IValueTaskSource.GetResult(Int16 token)

181

   at System.Net.Sockets.Socket.<ConnectAsync>g__WaitForConnectWithCancellation|285_0(AwaitableSocketAsyncEventArgs saea, ValueTask connectTask, CancellationToken cancellationToken)

182

   at Grpc.Net.Client.Balancer.Internal.SocketConnectivitySubchannelTransport.TryConnectAsync(ConnectContext context)

183

   --- End of inner exception stack trace ---

184

   at Grpc.Net.Client.Balancer.Internal.ConnectionManager.PickAsync(PickContext context, Boolean waitForReady, CancellationToken cancellationToken)

185

   at Grpc.Net.Client.Balancer.Internal.BalancerHttpHandler.SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)

186

   at Grpc.Net.Client.Internal.GrpcCall`2.RunCall(HttpRequestMessage request, Nullable`1 timeout)

187

   at Scout.Geofences.GrpcClient.GeocodingGeofencesGrpcSearcher.FindGeofencesAsync(GeofenceSearchRequest request) in /src/Platform/Components/Geofences/Scout.Geofences.GrpcClient/GeocodingGeofencesGrpcSearcher.cs:line 27

188

   at Scout.Geofences.GrpcClient.GeofencesGrpcSearcher.FindGeofencesAsync(SingleLocationGeofenceRequestInfo requestInfo) in /src/Platform/Components/Geofences/Scout.Geofences.GrpcClient/GeofencesGrpcSearcher.cs:line 111

189

   at Scout.Platform.Streaming.Reports.Reports.Rmwf.RmwfReportBuilder.GetGeofencesAsync(IReadOnlyList`1 locations) in /src/Platform/Streaming/Reports/Scout.Platform.Streaming.Reports/Reports/Rmwf/RmwfReportBuilder.cs:line 391

190

   at Scout.Platform.Streaming.Reports.Reports.Rmwf.RmwfReportBuilder.CreateFinalResultsAsync(ReportBuildRequest request, IReadOnlyCollection`1 results, RmwfReportSettings settings, CancellationToken token) in /src/Platform/Streaming/Reports/Scout.Platform.Streaming.Reports/Reports/Rmwf/RmwfReportBuilder.cs:line 253

