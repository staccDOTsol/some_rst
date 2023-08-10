a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/query_transformer/TimelineRankerQueryTransformer.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/query_transformer/TimelineRankerQueryTransformer.scala
==================================================

Change Range: -81,12 +78,14

Content:

::

index d5b29974b..e187a0aa0 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/query_transformer/TimelineRankerQueryTransformer.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/query_transformer/TimelineRankerQueryTransformer.scala
  
   import com.twitter.home_mixer.model.request.HasDeviceContext
  
   import com.twitter.home_mixer.product.scored_tweets.query_transformer.TimelineRankerQueryTransformer._
  
   import com.twitter.home_mixer.util.CachedScoredTweetsHelper
  
  +import com.twitter.home_mixer.util.earlybird.EarlybirdRequestUtil
  
   import com.twitter.product_mixer.core.model.common.identifier.CandidatePipelineIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.product_mixer.core.quality_factor.HasQualityFactorStatus
  
  -import com.twitter.timelinemixer.clients.timelineranker.EarlybirdScoringModels
  
  -import com.twitter.timelinemixer.clients.timelineranker.EarlybirdScoringModelsId
  
   import com.twitter.timelineranker.{model => tlr}
  
   import com.twitter.timelines.common.model.TweetKindOption
  
   import com.twitter.timelines.earlybird.common.options.EarlybirdOptions
  
     /**
  
      * Maximum number of results TLR should retrieve from each earlybird shard.
  
      */
  
  -  private val EarlybirdMaxResults = 200
  
  +  private val EarlybirdMaxResults = 300
  
   }
  
   
  
   trait TimelineRankerQueryTransformer[
  
     Query <: PipelineQuery with HasQualityFactorStatus with HasDeviceContext] {
  
     def maxTweetsToFetch: Int
  
  -  def sinceDuration: Duration
  
     def options: TweetKindOption.ValueSet = TweetKindOption.Default
  
     def candidateTweetSourceId: CandidateTweetSourceId.Value
  
  -  def skipVeryRecentTweets: Boolean
  
     def utegLikedByTweetsOptions(query: Query): Option[tlr.UtegLikedByTweetsOptions] = None
  
     def seedAuthorIds(query: Query): Option[Seq[Long]] = None
  
     def candidatePipelineIdentifier: CandidatePipelineIdentifier
  
     def earlybirdModels: Seq[EarlybirdScoringModelConfig] =
  
  -    EarlybirdScoringModels.fromEnum(EarlybirdScoringModelsId.UnifiedEngagementProd)
  
  -  def tensorflowModel: Option[String] = None
  
  +    EarlybirdRequestUtil.EarlybirdScoringModels.UnifiedEngagementProd
  
  +  def getTensorflowModel(query: Query): Option[String] = None
  
   
  
  -  def buildTimelineRankerQuery(query: Query): tlr.RecapQuery = {
  
  +  def buildTimelineRankerQuery(query: Query, sinceDuration: Duration): tlr.RecapQuery = {
  
       val sinceTime: Time = sinceDuration.ago
  
       val untilTime: Time = Time.now
  
   
  
       val deviceContext =
  
         query.deviceContext.map(_.toTimelineServiceDeviceContext(query.clientContext))
  
   
  
  +    val tensorflowModel = getTensorflowModel(query)
  
  +
  
       val earlyBirdOptions = EarlybirdOptions(
  
         maxNumHitsPerShard = EarlybirdMaxHits,
  
         maxNumResultsPerShard = EarlybirdMaxResults,
  
         models = earlybirdModels,
  
         authorScoreMap = authorScoreMap,
  
  -      skipVeryRecentTweets = skipVeryRecentTweets,
  
  +      skipVeryRecentTweets = true,
  
         tensorflowModel = tensorflowModel
  
       )
  
   
  
