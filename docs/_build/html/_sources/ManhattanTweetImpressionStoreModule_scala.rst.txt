a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ManhattanTweetImpressionStoreModule.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ManhattanTweetImpressionStoreModule.scala
==================================================

Change Range: -39,7 +45,7

Content:

::

index 1f6ae824a..c6782665a 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ManhattanTweetImpressionStoreModule.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/ManhattanTweetImpressionStoreModule.scala
  
   import com.twitter.finagle.mtls.authentication.ServiceIdentifier
  
   import com.twitter.finagle.stats.StatsReceiver
  
   import com.twitter.inject.TwitterModule
  
  +import com.twitter.inject.annotations.Flag
  
   import com.twitter.storage.client.manhattan.kv.Guarantee
  
   import com.twitter.storehaus_internal.manhattan.ManhattanClusters
  
   import com.twitter.timelines.clients.manhattan.mhv3.ManhattanClientBuilder
  
   import com.twitter.timelines.impressionstore.store.ManhattanTweetImpressionStoreClientConfig
  
   import com.twitter.timelines.impressionstore.store.ManhattanTweetImpressionStoreClient
  
  +import com.twitter.util.Duration
  
   import javax.inject.Singleton
  
   
  
   object ManhattanTweetImpressionStoreModule extends TwitterModule {
  
     private val StagingDataset = "timelines_tweet_impressions_staging"
  
     private val StatsScope = "manhattanTweetImpressionStoreClient"
  
     private val DefaultTTL = 2.days
  
  +  private final val Timeout = "mh_impression_store.timeout"
  
  +
  
  +  flag[Duration](Timeout, 150.millis, "Timeout per request")
  
   
  
     @Provides
  
     @Singleton
  
     def providesManhattanTweetImpressionStoreClient(
  
  +    @Flag(Timeout) timeout: Duration,
  
       serviceIdentifier: ServiceIdentifier,
  
       statsReceiver: StatsReceiver
  
     ): ManhattanTweetImpressionStoreClient = {
  
         dataset = dataset,
  
         statsScope = StatsScope,
  
         defaultGuarantee = Guarantee.SoftDcReadMyWrites,
  
  -      defaultMaxTimeout = 100.milliseconds,
  
  +      defaultMaxTimeout = timeout,
  
         maxRetryCount = 2,
  
         isReadOnly = false,
  
         serviceIdentifier = serviceIdentifier,
  
