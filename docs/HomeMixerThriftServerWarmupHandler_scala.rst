a/home-mixer/server/src/main/scala/com/twitter/home_mixer/HomeMixerThriftServerWarmupHandler.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/HomeMixerThriftServerWarmupHandler.scala
==================================================

Change Range: -3,7 +3,7

Content:

::

index df7001072..982b77487 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/HomeMixerThriftServerWarmupHandler.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/HomeMixerThriftServerWarmupHandler.scala
  
   import com.twitter.finagle.thrift.ClientId
  
   import com.twitter.finatra.thrift.routing.ThriftWarmup
  
   import com.twitter.home_mixer.{thriftscala => st}
  
  -import com.twitter.inject.Logging
  
  +import com.twitter.util.logging.Logging
  
   import com.twitter.inject.utils.Handler
  
   import com.twitter.product_mixer.core.{thriftscala => pt}
  
   import com.twitter.scrooge.Request
  
