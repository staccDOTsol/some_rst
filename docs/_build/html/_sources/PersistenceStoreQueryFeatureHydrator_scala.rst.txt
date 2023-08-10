a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/PersistenceStoreQueryFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/PersistenceStoreQueryFeatureHydrator.scala
==================================================

Change Range: -80,8 +92,19

Content:

::

index 10f42865c..b7aae811b 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/PersistenceStoreQueryFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/PersistenceStoreQueryFeatureHydrator.scala
  
   
  
   import com.twitter.conversions.DurationOps._
  
   import com.twitter.common_internal.analytics.twitter_client_user_agent_parser.UserAgent
  
  +import com.twitter.finagle.stats.StatsReceiver
  
   import com.twitter.home_mixer.model.HomeFeatures.PersistenceEntriesFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.ServedTweetIdsFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.ServedTweetPreviewIdsFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.WhoToFollowExcludedUserIdsFeature
  
   import com.twitter.home_mixer.model.request.FollowingProduct
  
   import com.twitter.home_mixer.model.request.ForYouProduct
  
   
  
   @Singleton
  
   case class PersistenceStoreQueryFeatureHydrator @Inject() (
  
  -  timelineResponseBatchesClient: TimelineResponseBatchesClient[TimelineResponseV3])
  
  +  timelineResponseBatchesClient: TimelineResponseBatchesClient[TimelineResponseV3],
  
  +  statsReceiver: StatsReceiver)
  
       extends QueryFeatureHydrator[PipelineQuery] {
  
   
  
     override val identifier: FeatureHydratorIdentifier = FeatureHydratorIdentifier("PersistenceStore")
  
   
  
  +  private val scopedStatsReceiver = statsReceiver.scope(getClass.getSimpleName)
  
  +  private val servedTweetIdsSizeStat = scopedStatsReceiver.stat("ServedTweetIdsSize")
  
  +
  
     private val WhoToFollowExcludedUserIdsLimit = 1000
  
  -  private val ServedTweetIdsDuration = 1.hour
  
  +  private val ServedTweetIdsDuration = 10.minutes
  
     private val ServedTweetIdsLimit = 100
  
  +  private val ServedTweetPreviewIdsDuration = 10.hours
  
  +  private val ServedTweetPreviewIdsLimit = 10
  
   
  
     override val features: Set[Feature[_, _]] =
  
  -    Set(ServedTweetIdsFeature, PersistenceEntriesFeature, WhoToFollowExcludedUserIdsFeature)
  
  +    Set(
  
  +      ServedTweetIdsFeature,
  
  +      ServedTweetPreviewIdsFeature,
  
  +      PersistenceEntriesFeature,
  
  +      WhoToFollowExcludedUserIdsFeature)
  
   
  
     private val supportedClients = Seq(
  
       ClientPlatform.IPhone,
  
               .flatMap(
  
                 _.entries.flatMap(_.tweetIds(includeSourceTweets = true)).take(ServedTweetIdsLimit))
  
   
  
  +          servedTweetIdsSizeStat.add(servedTweetIds.size)
  
  +
  
  +          val servedTweetPreviewIds = timelineResponses
  
  +            .filter(_.clientPlatform == clientPlatform)
  
  +            .filter(_.servedTime >= Time.now - ServedTweetPreviewIdsDuration)
  
  +            .sortBy(-_.servedTime.inMilliseconds)
  
  +            .flatMap(_.entries
  
  +              .filter(_.entityIdType == EntityIdType.TweetPreview)
  
  +              .flatMap(_.tweetIds(includeSourceTweets = true)).take(ServedTweetPreviewIdsLimit))
  
  +
  
             FeatureMapBuilder()
  
               .add(ServedTweetIdsFeature, servedTweetIds)
  
  +            .add(ServedTweetPreviewIdsFeature, servedTweetPreviewIds)
  
               .add(PersistenceEntriesFeature, timelineResponses)
  
               .add(WhoToFollowExcludedUserIdsFeature, whoToFollowUserIds)
  
               .build()
  
