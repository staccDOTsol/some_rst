a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ImpressionBloomFilterModule.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ImpressionBloomFilterModule.scala
==================================================

Change Range: -20,35 +22,38

Content:

::

index cb339a1d5..f37531483 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ImpressionBloomFilterModule.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ImpressionBloomFilterModule.scala
  
   import com.twitter.finagle.mtls.authentication.ServiceIdentifier
  
   import com.twitter.finagle.stats.StatsReceiver
  
   import com.twitter.inject.TwitterModule
  
  +import com.twitter.inject.annotations.Flag
  
   import com.twitter.storage.client.manhattan.kv.Guarantee
  
   import com.twitter.storehaus_internal.manhattan.ManhattanClusters
  
   import com.twitter.timelines.clients.manhattan.store._
  
  -import com.twitter.timelines.impressionstore.impressionbloomfilter.ImpressionBloomFilter
  
  +import com.twitter.timelines.impressionbloomfilter.{thriftscala => blm}
  
   import com.twitter.timelines.impressionstore.impressionbloomfilter.ImpressionBloomFilterManhattanKeyValueDescriptor
  
  +import com.twitter.util.Duration
  
   import javax.inject.Singleton
  
   
  
   object ImpressionBloomFilterModule extends TwitterModule {
  
     private val StagingDataset = "impression_bloom_filter_staging"
  
     private val ClientStatsScope = "tweetBloomFilterImpressionManhattanClient"
  
     private val DefaultTTL = 7.days
  
  +  private final val Timeout = "mh_impression_store_bloom_filter.timeout"
  
  +
  
  +  flag[Duration](Timeout, 150.millis, "Timeout per request")
  
   
  
     @Provides
  
     @Singleton
  
     def providesImpressionBloomFilter(
  
  +    @Flag(Timeout) timeout: Duration,
  
       serviceIdentifier: ServiceIdentifier,
  
       statsReceiver: StatsReceiver
  
  -  ): ImpressionBloomFilter = {
  
  +  ): ManhattanStoreClient[blm.ImpressionBloomFilterKey, blm.ImpressionBloomFilterSeq] = {
  
       val (appId, dataset) = serviceIdentifier.environment.toLowerCase match {
  
         case "prod" => (ProdAppId, ProdDataset)
  
         case _ => (StagingAppId, StagingDataset)
  
       }
  
   
  
  -    implicit val manhattanKeyValueDescriptor = ImpressionBloomFilterManhattanKeyValueDescriptor(
  
  -      dataset = dataset,
  
  -      ttl = DefaultTTL
  
  -    )
  
  +    implicit val manhattanKeyValueDescriptor: ImpressionBloomFilterManhattanKeyValueDescriptor =
  
  +      ImpressionBloomFilterManhattanKeyValueDescriptor(
  
  +        dataset = dataset,
  
  +        ttl = DefaultTTL
  
  +      )
  
   
  
  -    val manhattanClient = ManhattanStoreClientBuilder.buildManhattanClient(
  
  +    ManhattanStoreClientBuilder.buildManhattanClient(
  
         serviceIdentifier = serviceIdentifier,
  
         cluster = ManhattanClusters.nash,
  
         appId = appId,
  
  -      defaultMaxTimeout = 100.milliseconds,
  
  +      defaultMaxTimeout = timeout,
  
         maxRetryCount = 2,
  
         defaultGuarantee = Some(Guarantee.SoftDcReadMyWrites),
  
         isReadOnly = false,
  
         statsScope = ClientStatsScope,
  
         statsReceiver = statsReceiver
  
       )
  
  -
  
  -    ImpressionBloomFilter(manhattanClient)
  
     }
  
   }
  
