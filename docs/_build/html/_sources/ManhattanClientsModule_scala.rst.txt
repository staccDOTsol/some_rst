a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ManhattanClientsModule.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ManhattanClientsModule.scala
==================================================

Change Range: -13,44 +14,28

Content:

::

index 5bdbf9e97..fc0e282af 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ManhattanClientsModule.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ManhattanClientsModule.scala
  
   package com.twitter.home_mixer.module
  
   
  
   import com.google.inject.Provides
  
  +import com.twitter.conversions.DurationOps._
  
   import com.twitter.finagle.mtls.authentication.ServiceIdentifier
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.RealGraphManhattanEndpoint
  
  -import com.twitter.home_mixer.param.HomeMixerInjectionNames.UserMetadataManhattanEndpoint
  
   import com.twitter.inject.TwitterModule
  
  +import com.twitter.inject.annotations.Flag
  
   import com.twitter.storage.client.manhattan.kv._
  
   import com.twitter.timelines.config.ConfigUtils
  
   import com.twitter.util.Duration
  
   
  
   object ManhattanClientsModule extends TwitterModule with ConfigUtils {
  
   
  
  -  private val starbuckDest: String = "/s/manhattan/starbuck.native-thrift"
  
  -  private val apolloDest: String = "/s/manhattan/apollo.native-thrift"
  
  +  private val ApolloDest = "/s/manhattan/apollo.native-thrift"
  
  +  private final val Timeout = "mh_real_graph.timeout"
  
  +
  
  +  flag[Duration](Timeout, 150.millis, "Timeout total")
  
   
  
     @Provides
  
     @Singleton
  
     @Named(RealGraphManhattanEndpoint)
  
     def providesRealGraphManhattanEndpoint(
  
  +    @Flag(Timeout) timeout: Duration,
  
       serviceIdentifier: ServiceIdentifier
  
     ): ManhattanKVEndpoint = {
  
       lazy val client = ManhattanKVClient(
  
         appId = "real_graph",
  
  -      dest = apolloDest,
  
  +      dest = ApolloDest,
  
         mtlsParams = ManhattanKVClientMtlsParams(serviceIdentifier = serviceIdentifier),
  
         label = "real-graph-data"
  
       )
  
   
  
       ManhattanKVEndpointBuilder(client)
  
         .maxRetryCount(2)
  
  -      .defaultMaxTimeout(Duration.fromMilliseconds(100))
  
  -      .build()
  
  -  }
  
  -
  
  -  @Provides
  
  -  @Singleton
  
  -  @Named(UserMetadataManhattanEndpoint)
  
  -  def providesUserMetadataManhattanEndpoint(
  
  -    serviceIdentifier: ServiceIdentifier
  
  -  ): ManhattanKVEndpoint = {
  
  -    val client = ManhattanKVClient(
  
  -      appId = "user_metadata",
  
  -      dest = starbuckDest,
  
  -      mtlsParams = ManhattanKVClientMtlsParams(serviceIdentifier = serviceIdentifier),
  
  -      label = "user-metadata"
  
  -    )
  
  -
  
  -    ManhattanKVEndpointBuilder(client)
  
  -      .maxRetryCount(1)
  
  -      .defaultMaxTimeout(Duration.fromMilliseconds(70))
  
  +      .defaultMaxTimeout(timeout)
  
         .build()
  
     }
  
   }
  
