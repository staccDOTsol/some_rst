a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/HomeNaviModelClientModule.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/NaviModelClientModule.scala
==================================================

Change Range: -23,10 +22,9

Content:

::

similarity index 88%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/module/HomeNaviModelClientModule.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/module/NaviModelClientModule.scala
  
  index df70592c2..60d580a73 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/HomeNaviModelClientModule.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/NaviModelClientModule.scala
  
   import com.twitter.timelines.clients.predictionservice.PredictionGRPCService
  
   import com.twitter.util.Duration
  
   import io.grpc.ManagedChannel
  
  -
  
   import javax.inject.Singleton
  
   
  
  -object HomeNaviModelClientModule extends TwitterModule {
  
  +object NaviModelClientModule extends TwitterModule {
  
   
  
     @Singleton
  
     @Provides
  
       //  Wily path to the ML Model service (e.g. /s/ml-serving/navi-explore-ranker).
  
       val modelPath = "/s/ml-serving/navi_home_recap_onnx"
  
   
  
  -    // timeout for prediction service requests.
  
  -    val MaxPredictionTimeoutMs: Duration = 300.millis
  
  +    val MaxPredictionTimeoutMs: Duration = 500.millis
  
       val ConnectTimeoutMs: Duration = 200.millis
  
  -    val AcquisitionTimeoutMs: Duration = 20000.millis
  
  +    val AcquisitionTimeoutMs: Duration = 500.millis
  
       val MaxRetryAttempts: Int = 2
  
   
  
       val client = Http.client
  
