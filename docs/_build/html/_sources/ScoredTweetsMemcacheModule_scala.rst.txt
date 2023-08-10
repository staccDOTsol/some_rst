a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ScoredTweetsMemcacheModule.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ScoredTweetsMemcacheModule.scala
==================================================

Change Range: -49,17 +51,11

Content:

::

index b8ac940b1..4bed31c5c 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ScoredTweetsMemcacheModule.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ScoredTweetsMemcacheModule.scala
  
   import com.twitter.servo.cache.FinagleMemcache
  
   import com.twitter.servo.cache.KeyTransformer
  
   import com.twitter.servo.cache.KeyValueTransformingTtlCache
  
  -import com.twitter.servo.cache.ObservableTtlCache
  
   import com.twitter.servo.cache.Serializer
  
   import com.twitter.servo.cache.ThriftSerializer
  
   import com.twitter.servo.cache.TtlCache
  
     private val ScopeName = "ScoredTweetsCache"
  
     private val ProdDestName = "/srv#/prod/local/cache/home_scored_tweets:twemcaches"
  
     private val StagingDestName = "/srv#/test/local/cache/twemcache_home_scored_tweets:twemcaches"
  
  -  private val cachedScoredTweetsSerializer: Serializer[t.CachedScoredTweets] =
  
  -    new ThriftSerializer[t.CachedScoredTweets](t.CachedScoredTweets, new TCompactProtocol.Factory())
  
  +  private val scoredTweetsSerializer: Serializer[t.ScoredTweetsResponse] =
  
  +    new ThriftSerializer[t.ScoredTweetsResponse](
  
  +      t.ScoredTweetsResponse,
  
  +      new TCompactProtocol.Factory())
  
     private val userIdKeyTransformer: KeyTransformer[UserId] = (userId: UserId) => userId.toString
  
   
  
     @Singleton
  
     def providesScoredTweetsCache(
  
       serviceIdentifier: ServiceIdentifier,
  
       statsReceiver: StatsReceiver
  
  -  ): TtlCache[UserId, t.CachedScoredTweets] = {
  
  +  ): TtlCache[UserId, t.ScoredTweetsResponse] = {
  
       val destName = serviceIdentifier.environment.toLowerCase match {
  
         case "prod" => ProdDestName
  
         case _ => StagingDestName
  
       val client = MemcachedClientBuilder.buildMemcachedClient(
  
         destName = destName,
  
         numTries = 2,
  
  +      numConnections = 1,
  
         requestTimeout = 200.milliseconds,
  
         globalTimeout = 400.milliseconds,
  
         connectTimeout = 100.milliseconds,
  
         statsReceiver = statsReceiver.scope(ScopeName)
  
       )
  
       val underlyingCache = new FinagleMemcache(client)
  
  -    val baseCache: KeyValueTransformingTtlCache[UserId, String, t.CachedScoredTweets, Array[Byte]] =
  
  -      new KeyValueTransformingTtlCache(
  
  -        underlyingCache = underlyingCache,
  
  -        transformer = cachedScoredTweetsSerializer,
  
  -        underlyingKey = userIdKeyTransformer
  
  -      )
  
  -    ObservableTtlCache(
  
  -      underlyingCache = baseCache,
  
  -      statsReceiver = statsReceiver,
  
  -      windowSize = 1000L,
  
  -      name = ScopeName
  
  +
  
  +    new KeyValueTransformingTtlCache(
  
  +      underlyingCache = underlyingCache,
  
  +      transformer = scoredTweetsSerializer,
  
  +      underlyingKey = userIdKeyTransformer
  
       )
  
     }
  
   }
  
