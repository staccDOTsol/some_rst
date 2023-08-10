a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/ReplyFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/ReplyFeatureHydrator.scala
==================================================

Change Range: -94,16 +128,28

Content:

::

similarity index 71%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/ReplyFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/ReplyFeatureHydrator.scala
  
  index 5833838f1..80d857a26 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/ReplyFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/ReplyFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
   import com.twitter.finagle.stats.StatsReceiver
  
  +import com.twitter.home_mixer.model.ContentFeatures
  
   import com.twitter.home_mixer.model.HomeFeatures._
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.content.InReplyToContentFeatureAdapter
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.adapters.earlybird.InReplyToEarlybirdAdapter
  
   import com.twitter.home_mixer.util.ReplyRetweetUtil
  
  +import com.twitter.ml.api.DataRecord
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.feature.Feature
  
  +import com.twitter.product_mixer.core.feature.FeatureWithDefaultOnFailure
  
  +import com.twitter.product_mixer.core.feature.datarecord.DataRecordInAFeature
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMapBuilder
  
   import com.twitter.product_mixer.core.functional_component.feature_hydrator.BulkCandidateFeatureHydrator
  
   import com.twitter.product_mixer.core.model.common.CandidateWithFeatures
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.search.common.features.thriftscala.ThriftTweetFeatures
  
   import com.twitter.snowflake.id.SnowflakeId
  
   import com.twitter.stitch.Stitch
  
   import com.twitter.timelines.conversation_features.v1.thriftscala.ConversationFeatures
  
  +import com.twitter.timelines.conversation_features.{thriftscala => cf}
  
  +import com.twitter.timelines.prediction.adapters.conversation_features.ConversationFeaturesAdapter
  
   import com.twitter.util.Duration
  
   import com.twitter.util.Time
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
  +import scala.collection.JavaConverters._
  
   
  
   object InReplyToTweetHydratedEarlybirdFeature
  
       extends Feature[TweetCandidate, Option[ThriftTweetFeatures]]
  
   
  
  +object ConversationDataRecordFeature
  
  +    extends DataRecordInAFeature[TweetCandidate]
  
  +    with FeatureWithDefaultOnFailure[TweetCandidate, DataRecord] {
  
  +  override def defaultValue: DataRecord = new DataRecord()
  
  +}
  
  +
  
  +object InReplyToEarlybirdDataRecordFeature
  
  +    extends DataRecordInAFeature[TweetCandidate]
  
  +    with FeatureWithDefaultOnFailure[TweetCandidate, DataRecord] {
  
  +  override def defaultValue: DataRecord = new DataRecord()
  
  +}
  
  +
  
  +object InReplyToTweetypieContentDataRecordFeature
  
  +    extends DataRecordInAFeature[TweetCandidate]
  
  +    with FeatureWithDefaultOnFailure[TweetCandidate, DataRecord] {
  
  +  override def defaultValue: DataRecord = new DataRecord()
  
  +}
  
  +
  
   /**
  
    * The purpose of this hydrator is to
  
    * 1) hydrate simple features into replies and their ancestor tweets
  
     override val identifier: FeatureHydratorIdentifier = FeatureHydratorIdentifier("ReplyTweet")
  
   
  
     override val features: Set[Feature[_, _]] = Set(
  
  -    ConversationFeature,
  
  -    InReplyToTweetHydratedEarlybirdFeature
  
  +    ConversationDataRecordFeature,
  
  +    InReplyToTweetHydratedEarlybirdFeature,
  
  +    InReplyToEarlybirdDataRecordFeature,
  
  +    InReplyToTweetypieContentDataRecordFeature
  
     )
  
   
  
  +  private val defaulDataRecord: DataRecord = new DataRecord()
  
  +
  
     private val DefaultFeatureMap = FeatureMapBuilder()
  
  -    .add(ConversationFeature, None)
  
  +    .add(ConversationDataRecordFeature, defaulDataRecord)
  
       .add(InReplyToTweetHydratedEarlybirdFeature, None)
  
  +    .add(InReplyToEarlybirdDataRecordFeature, defaulDataRecord)
  
  +    .add(InReplyToTweetypieContentDataRecordFeature, defaulDataRecord)
  
       .build()
  
   
  
     private val scopedStatsReceiver = statsReceiver.scope(getClass.getSimpleName)
  
     override def apply(
  
       query: PipelineQuery,
  
       candidates: Seq[CandidateWithFeatures[TweetCandidate]]
  
  -  ): Stitch[Seq[FeatureMap]] = {
  
  +  ): Stitch[Seq[FeatureMap]] = OffloadFuturePools.offload {
  
       val replyToInReplyToTweetMap =
  
         ReplyRetweetUtil.replyTweetIdToInReplyToTweetMap(candidates)
  
       val candidatesWithRepliesHydrated = candidates.map { candidate =>
  
                 inReplyToTweetEarlyBirdFeature,
  
                 updatedReplyConversationFeatures))
  
       }
  
  -    Stitch.value(
  
  -      candidatesWithRepliesAndAncestorTweetsHydrated.map {
  
  -        case (candidate, inReplyToTweetEarlyBirdFeature, updatedConversationFeatures) =>
  
  -          FeatureMapBuilder()
  
  -            .add(ConversationFeature, updatedConversationFeatures)
  
  -            .add(InReplyToTweetHydratedEarlybirdFeature, inReplyToTweetEarlyBirdFeature)
  
  -            .build()
  
  -        case _ => DefaultFeatureMap
  
  -      }
  
  -    )
  
  +
  
  +    candidatesWithRepliesAndAncestorTweetsHydrated.map {
  
  +      case (candidate, inReplyToTweetEarlyBirdFeature, updatedConversationFeatures) =>
  
  +        val conversationDataRecordFeature = updatedConversationFeatures
  
  +          .map(f => ConversationFeaturesAdapter.adaptToDataRecord(cf.ConversationFeatures.V1(f)))
  
  +          .getOrElse(defaulDataRecord)
  
  +
  
  +        val inReplyToEarlybirdDataRecord =
  
  +          InReplyToEarlybirdAdapter
  
  +            .adaptToDataRecords(inReplyToTweetEarlyBirdFeature).asScala.head
  
  +        val inReplyToContentDataRecord = InReplyToContentFeatureAdapter
  
  +          .adaptToDataRecords(
  
  +            inReplyToTweetEarlyBirdFeature.map(ContentFeatures.fromThrift)).asScala.head
  
  +
  
  +        FeatureMapBuilder()
  
  +          .add(ConversationDataRecordFeature, conversationDataRecordFeature)
  
  +          .add(InReplyToTweetHydratedEarlybirdFeature, inReplyToTweetEarlyBirdFeature)
  
  +          .add(InReplyToEarlybirdDataRecordFeature, inReplyToEarlybirdDataRecord)
  
  +          .add(InReplyToTweetypieContentDataRecordFeature, inReplyToContentDataRecord)
  
  +          .build()
  
  +      case _ => DefaultFeatureMap
  
  +    }
  
     }
  
   
  
     private def hydratedReplyCandidate(
  
