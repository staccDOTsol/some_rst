a/home-mixer/server/src/main/scala/com/twitter/home_mixer/HomeMixerHttpServerWarmupHandler.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/HomeMixerHttpServerWarmupHandler.scala
==================================================

Change Range: -2,7 +2,7

Content:

::

index 16a9d9c58..e27133b23 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/HomeMixerHttpServerWarmupHandler.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/HomeMixerHttpServerWarmupHandler.scala
  
   
  
   import com.twitter.finatra.http.routing.HttpWarmup
  
   import com.twitter.finatra.httpclient.RequestBuilder._
  
  -import com.twitter.inject.Logging
  
  +import com.twitter.util.logging.Logging
  
   import com.twitter.inject.utils.Handler
  
   import com.twitter.util.Try
  
   import javax.inject.Inject
  
