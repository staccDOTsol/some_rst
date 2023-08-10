a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/MemcachedFeatureRepositoryModule.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/MemcachedFeatureRepositoryModule.scala
==================================================

Change Range: -91,9 +94,10

Content:

::

index 2f5ce1f5d..8afafbfb7 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/MemcachedFeatureRepositoryModule.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/MemcachedFeatureRepositoryModule.scala
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.HomeAuthorFeaturesCacheClient
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.RealTimeInteractionGraphUserVertexClient
  
   import com.twitter.home_mixer.param.HomeMixerInjectionNames.TimelinesRealTimeAggregateClient
  
  -import com.twitter.home_mixer.param.HomeMixerInjectionNames.TwhinAuthorFollow20200101FeatureCacheClient
  
  +import com.twitter.home_mixer.param.HomeMixerInjectionNames.TwhinAuthorFollowFeatureCacheClient
  
   import com.twitter.inject.TwitterModule
  
   import com.twitter.product_mixer.shared_library.memcached_client.MemcachedClientBuilder
  
   import com.twitter.servo.cache.FinagleMemcacheFactory
  
       statsReceiver: StatsReceiver
  
     ): Memcache = {
  
       val rawClient = MemcachedClientBuilder.buildRawMemcachedClient(
  
  -      numTries = 1,
  
  -      requestTimeout = 150.milliseconds,
  
  -      globalTimeout = 150.milliseconds,
  
  +      numTries = 3,
  
  +      numConnections = 1,
  
  +      requestTimeout = 100.milliseconds,
  
  +      globalTimeout = 300.milliseconds,
  
         connectTimeout = 200.milliseconds,
  
         acquisitionTimeout = 200.milliseconds,
  
         serviceIdentifier = serviceIdentifier,
  
       statsReceiver: StatsReceiver
  
     ): Memcache = {
  
       val cacheClient = MemcachedClientBuilder.buildRawMemcachedClient(
  
  -      numTries = 1,
  
  -      requestTimeout = 50.milliseconds,
  
  -      globalTimeout = 50.milliseconds,
  
  +      numTries = 2,
  
  +      numConnections = 1,
  
  +      requestTimeout = 150.milliseconds,
  
  +      globalTimeout = 300.milliseconds,
  
         connectTimeout = 200.milliseconds,
  
         acquisitionTimeout = 200.milliseconds,
  
         serviceIdentifier = serviceIdentifier,
  
   
  
     @Provides
  
     @Singleton
  
  -  @Named(TwhinAuthorFollow20200101FeatureCacheClient)
  
  -  def providesTwhinAuthorFollow20200101FeatureCacheClient(
  
  +  @Named(TwhinAuthorFollowFeatureCacheClient)
  
  +  def providesTwhinAuthorFollowFeatureCacheClient(
  
       serviceIdentifier: ServiceIdentifier,
  
       statsReceiver: StatsReceiver
  
     ): Memcache = {
  
       val cacheClient = MemcachedClientBuilder.buildRawMemcachedClient(
  
  -      numTries = 1,
  
  -      requestTimeout = 50.milliseconds,
  
  -      globalTimeout = 50.milliseconds,
  
  +      numTries = 2,
  
  +      numConnections = 1,
  
  +      requestTimeout = 150.milliseconds,
  
  +      globalTimeout = 300.milliseconds,
  
         connectTimeout = 200.milliseconds,
  
         acquisitionTimeout = 200.milliseconds,
  
         serviceIdentifier = serviceIdentifier,
  
       statsReceiver: StatsReceiver
  
     ): Memcache = {
  
       val cacheClient = MemcachedClientBuilder.buildRawMemcachedClient(
  
  -      numTries = 1,
  
  -      requestTimeout = 100.milliseconds,
  
  -      globalTimeout = 100.milliseconds,
  
  +      numTries = 2,
  
  +      numConnections = 1,
  
  +      requestTimeout = 150.milliseconds,
  
  +      globalTimeout = 300.milliseconds,
  
         connectTimeout = 200.milliseconds,
  
         acquisitionTimeout = 200.milliseconds,
  
         serviceIdentifier = serviceIdentifier,
  
