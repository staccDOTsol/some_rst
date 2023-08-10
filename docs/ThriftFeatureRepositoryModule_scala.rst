a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ThriftFeatureRepositoryModule.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ThriftFeatureRepositoryModule.scala
==================================================

Change Range: -281,10 +279,12

Content:

::

index 5a2a8701a..dcbf62451 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ThriftFeatureRepositoryModule.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ThriftFeatureRepositoryModule.scala
  
           label = "interests",
  
           statsReceiver = statsReceiver,
  
           idempotency = Idempotent(1.percent),
  
  -        timeoutPerRequest = 100.milliseconds,
  
  -        timeoutTotal = 100.milliseconds
  
  +        timeoutPerRequest = 350.milliseconds,
  
  +        timeoutTotal = 350.milliseconds
  
         )
  
     }
  
   
  
           label = "tweetypie-content-repo",
  
           statsReceiver = statsReceiver,
  
           idempotency = Idempotent(1.percent),
  
  -        timeoutPerRequest = 150.milliseconds,
  
  -        timeoutTotal = 250.milliseconds
  
  +        timeoutPerRequest = 300.milliseconds,
  
  +        timeoutTotal = 500.milliseconds
  
         )
  
   
  
       def lookup(tweetIds: Seq[Long]): Future[Seq[Option[tp.Tweet]]] = {
  
           includeRetweetedTweet = false,
  
           includeQuotedTweet = false,
  
           forUserId = None,
  
  -        // Service needs to be whitelisted
  
  -        // We rely on the VF at the end of serving. No need to filter now.
  
           safetyLevel = Some(sp.SafetyLevel.FilterNone),
  
           visibilityPolicy = tp.TweetVisibilityPolicy.NoFiltering
  
         )
  
  -      val request = tp.GetTweetFieldsRequest(
  
  -        tweetIds = tweetIds,
  
  -        options = getTweetFieldsOptions
  
  -      )
  
  +
  
  +      val request = tp.GetTweetFieldsRequest(tweetIds = tweetIds, options = getTweetFieldsOptions)
  
  +
  
         client.getTweetFields(request).map { results =>
  
           results.map {
  
             case tp.GetTweetFieldsResult(_, tp.TweetFieldsResultState.Found(found), _, _) =>
  
   
  
       val keyValueRepository = toRepositoryBatch(lookup, chunkSize = 20)
  
   
  
  -    // Cache
  
       val cacheClient = MemcachedClientBuilder.buildRawMemcachedClient(
  
         numTries = 1,
  
  -      requestTimeout = 100.milliseconds,
  
  -      globalTimeout = 100.milliseconds,
  
  +      numConnections = 1,
  
  +      requestTimeout = 200.milliseconds,
  
  +      globalTimeout = 200.milliseconds,
  
         connectTimeout = 200.milliseconds,
  
         acquisitionTimeout = 200.milliseconds,
  
         serviceIdentifier = serviceIdentifier,
  
         statsReceiver = statsReceiver
  
       )
  
  +
  
       val finagleMemcacheFactory =
  
         FinagleMemcacheFactory(cacheClient, "/s/cache/home_content_features:twemcaches")
  
       val cacheValueTransformer =
  
         tweetIds: Seq[Long],
  
         viewerId: Long
  
       ): Future[Seq[Option[eb.ThriftSearchResult]]] = {
  
  -      val request = EarlybirdRequestUtil.getTweetsEBFeaturesRequest(
  
  +      val request = EarlybirdRequestUtil.getTweetsFeaturesRequest(
  
           userId = Some(viewerId),
  
           tweetIds = Some(tweetIds),
  
  -        clientId = Some(clientId.name)
  
  +        clientId = Some(clientId.name),
  
  +        authorScoreMap = None,
  
  +        tensorflowModel = Some("timelines_rectweet_replica")
  
         )
  
   
  
         client
  
