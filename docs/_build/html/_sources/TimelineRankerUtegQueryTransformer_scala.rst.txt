a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/query_transformer/TimelineRankerUtegQueryTransformer.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/query_transformer/TimelineRankerUtegQueryTransformer.scala
==================================================

Change Range: -55,5 +55,5

Content:

::

index 03a63acf9..ea051f331 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/query_transformer/TimelineRankerUtegQueryTransformer.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/query_transformer/TimelineRankerUtegQueryTransformer.scala
  
   import com.twitter.conversions.DurationOps._
  
   import com.twitter.home_mixer.model.HomeFeatures.RealGraphInNetworkScoresFeature
  
   import com.twitter.home_mixer.model.request.HasDeviceContext
  
  +import com.twitter.home_mixer.product.scored_tweets.param.ScoredTweetsParam
  
   import com.twitter.home_mixer.product.scored_tweets.query_transformer.TimelineRankerUtegQueryTransformer._
  
  +import com.twitter.home_mixer.util.earlybird.EarlybirdRequestUtil
  
   import com.twitter.product_mixer.core.functional_component.transformer.CandidatePipelineQueryTransformer
  
   import com.twitter.product_mixer.core.model.common.identifier.CandidatePipelineIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.product_mixer.core.quality_factor.HasQualityFactorStatus
  
  -import com.twitter.timelinemixer.clients.timelineranker.EarlybirdScoringModels
  
  -import com.twitter.timelinemixer.clients.timelineranker.EarlybirdScoringModelsId
  
   import com.twitter.timelineranker.{model => tlr}
  
   import com.twitter.timelineranker.{thriftscala => t}
  
   import com.twitter.timelines.common.model.TweetKindOption
  
  +import com.twitter.timelines.earlybird.common.options.EarlybirdScoringModelConfig
  
   import com.twitter.timelines.model.UserId
  
   import com.twitter.timelines.model.candidate.CandidateTweetSourceId
  
  -import com.twitter.util.Duration
  
   
  
   object TimelineRankerUtegQueryTransformer {
  
     private val SinceDuration = 24.hours
  
  -  private val MaxTweetsToFetch = 500
  
  +  private val MaxTweetsToFetch = 300
  
     private val MaxUtegCandidates = 800
  
   
  
  -  private val TensorflowModel = "timelines_rectweet_replica"
  
  +  private val tweetKindOptions =
  
  +    TweetKindOption(includeOriginalTweetsAndQuotes = true, includeReplies = true)
  
   
  
  -  private val tweetKindOptions = TweetKindOption(includeReplies = true)
  
  -
  
  -  def utegEarlybirdModels =
  
  -    EarlybirdScoringModels.fromEnum(EarlybirdScoringModelsId.UnifiedEngagementRectweet)
  
  +  def utegEarlybirdModels: Seq[EarlybirdScoringModelConfig] =
  
  +    EarlybirdRequestUtil.EarlybirdScoringModels.UnifiedEngagementRectweet
  
   }
  
   
  
   case class TimelineRankerUtegQueryTransformer[
  
     Query <: PipelineQuery with HasQualityFactorStatus with HasDeviceContext
  
   ](
  
     override val candidatePipelineIdentifier: CandidatePipelineIdentifier,
  
  -  override val maxTweetsToFetch: Int = MaxTweetsToFetch,
  
  -  override val sinceDuration: Duration = SinceDuration)
  
  +  override val maxTweetsToFetch: Int = MaxTweetsToFetch)
  
       extends CandidatePipelineQueryTransformer[Query, t.UtegLikedByTweetsQuery]
  
       with TimelineRankerQueryTransformer[Query] {
  
   
  
     override val candidateTweetSourceId = CandidateTweetSourceId.RecommendedTweet
  
  -  override val skipVeryRecentTweets = true
  
  +  override val options = tweetKindOptions
  
     override val earlybirdModels = utegEarlybirdModels
  
  -  override val tensorflowModel = Some(TensorflowModel)
  
  +  override def getTensorflowModel(query: Query): Option[String] = {
  
  +    Some(query.params(ScoredTweetsParam.EarlybirdTensorflowModel.UtegParam))
  
  +  }
  
   
  
     override def utegLikedByTweetsOptions(input: Query): Option[tlr.UtegLikedByTweetsOptions] = Some(
  
       tlr.UtegLikedByTweetsOptions(
  
     )
  
   
  
     override def transform(input: Query): t.UtegLikedByTweetsQuery =
  
  -    buildTimelineRankerQuery(input).toThriftUtegLikedByTweetsQuery
  
  +    buildTimelineRankerQuery(input, SinceDuration).toThriftUtegLikedByTweetsQuery
  
   }
  
