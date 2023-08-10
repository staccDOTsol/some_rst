a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/TweetyPieClientModule.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/TweetyPieClientModule.scala
==================================================

Change Range: -42,10 +50,14

Content:

::

index 84b634cc3..1eb49206c 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/TweetyPieClientModule.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/TweetyPieClientModule.scala
  
   import com.twitter.finagle.thriftmux.MethodBuilder
  
   import com.twitter.finatra.mtls.thriftmux.modules.MtlsClient
  
   import com.twitter.inject.Injector
  
  +import com.twitter.inject.annotations.Flags
  
   import com.twitter.inject.thrift.modules.ThriftMethodBuilderClientModule
  
   import com.twitter.stitch.tweetypie.TweetyPie
  
   import com.twitter.tweetypie.thriftscala.TweetService
  
   import javax.inject.Singleton
  
   
  
   /**
  
  - * Idempotent TweetyPie Thrift and Stitch client.
  
  + * Idempotent Tweetypie Thrift and Stitch client.
  
    */
  
  -object TweetyPieClientModule
  
  +object TweetypieClientModule
  
       extends ThriftMethodBuilderClientModule[
  
         TweetService.ServicePerEndpoint,
  
         TweetService.MethodPerEndpoint
  
       ]
  
       with MtlsClient {
  
  +
  
  +  private val TimeoutRequest = "tweetypie.timeout_request"
  
  +  private val TimeoutTotal = "tweetypie.timeout_total"
  
  +
  
  +  flag[Duration](TimeoutRequest, 1000.millis, "Timeout per request")
  
  +  flag[Duration](TimeoutTotal, 1000.millis, "Total timeout")
  
  +
  
     override val label: String = "tweetypie"
  
     override val dest: String = "/s/tweetypie/tweetypie"
  
   
  
     override protected def configureMethodBuilder(
  
       injector: Injector,
  
       methodBuilder: MethodBuilder
  
  -  ): MethodBuilder =
  
  +  ): MethodBuilder = {
  
  +    val timeoutRequest = injector.instance[Duration](Flags.named(TimeoutRequest))
  
  +    val timeoutTotal = injector.instance[Duration](Flags.named(TimeoutTotal))
  
  +
  
       methodBuilder
  
  -      .withTimeoutPerRequest(500.milliseconds)
  
  -      .withTimeoutTotal(500.milliseconds)
  
  +      .withTimeoutPerRequest(timeoutRequest)
  
  +      .withTimeoutTotal(timeoutTotal)
  
  +  }
  
   
  
  -  override protected def sessionAcquisitionTimeout: Duration = 250.milliseconds
  
  +  override protected def sessionAcquisitionTimeout: Duration = 500.millis
  
   }
  
