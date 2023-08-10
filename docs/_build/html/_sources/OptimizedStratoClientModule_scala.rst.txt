a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/OptimizedStratoClientModule.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/OptimizedStratoClientModule.scala
==================================================

Change Range: -35,7 +35,7

Content:

::

index 7575685f1..b9e315acc 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/OptimizedStratoClientModule.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/OptimizedStratoClientModule.scala
  
   
  
   object OptimizedStratoClientModule extends TwitterModule {
  
   
  
  -  private val ModerateStratoServerClientRequestTimeout = 150.millis
  
  +  private val ModerateStratoServerClientRequestTimeout = 500.millis
  
   
  
     private val DefaultRetryPartialFunction: PartialFunction[Try[Nothing], Boolean] =
  
       RetryPolicy.TimeoutAndWriteExceptionsOnly
  
     ): Client = {
  
       Strato.client
  
         .withMutualTls(serviceIdentifier, opportunisticLevel = OpportunisticTls.Required)
  
  -      .withSession.acquisitionTimeout(150.milliseconds)
  
  +      .withSession.acquisitionTimeout(500.milliseconds)
  
         .withRequestTimeout(ModerateStratoServerClientRequestTimeout)
  
         .withPerRequestTimeout(ModerateStratoServerClientRequestTimeout)
  
         .withRpcBatchSize(5)
  
