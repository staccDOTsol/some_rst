a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/query_transformer/TimelineRankerInNetworkQueryTransformer.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/query_transformer/TimelineRankerInNetworkQueryTransformer.scala
==================================================

Change Range: -22,21 +25,39

Content:

::

index bec8f74f2..4514dc2c4 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/query_transformer/TimelineRankerInNetworkQueryTransformer.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/query_transformer/TimelineRankerInNetworkQueryTransformer.scala
  
   package com.twitter.home_mixer.product.scored_tweets.query_transformer
  
   
  
   import com.twitter.conversions.DurationOps._
  
  +import com.twitter.core_workflows.user_model.{thriftscala => um}
  
  +import com.twitter.home_mixer.model.HomeFeatures.UserStateFeature
  
   import com.twitter.home_mixer.model.request.HasDeviceContext
  
  +import com.twitter.home_mixer.product.scored_tweets.param.ScoredTweetsParam
  
   import com.twitter.home_mixer.product.scored_tweets.query_transformer.TimelineRankerInNetworkQueryTransformer._
  
   import com.twitter.product_mixer.core.functional_component.transformer.CandidatePipelineQueryTransformer
  
   import com.twitter.product_mixer.core.model.common.identifier.CandidatePipelineIdentifier
  
   import com.twitter.timelineranker.{thriftscala => t}
  
   import com.twitter.timelines.common.model.TweetKindOption
  
   import com.twitter.timelines.model.candidate.CandidateTweetSourceId
  
  -import com.twitter.util.Duration
  
   
  
   object TimelineRankerInNetworkQueryTransformer {
  
  -  private val SinceDuration = 24.hours
  
  -  private val MaxTweetsToFetch = 500
  
  +  private val DefaultSinceDuration = 24.hours
  
  +  private val ExpandedSinceDuration = 48.hours
  
  +  private val MaxTweetsToFetch = 600
  
   
  
     private val tweetKindOptions: TweetKindOption.ValueSet = TweetKindOption(
  
       includeReplies = true,
  
       includeOriginalTweetsAndQuotes = true,
  
       includeExtendedReplies = true
  
     )
  
  +
  
  +  private val UserStatesForExtendedSinceDuration: Set[um.UserState] = Set(
  
  +    um.UserState.Light,
  
  +    um.UserState.MediumNonTweeter,
  
  +    um.UserState.MediumTweeter,
  
  +    um.UserState.NearZero,
  
  +    um.UserState.New,
  
  +    um.UserState.VeryLight
  
  +  )
  
   }
  
   
  
   case class TimelineRankerInNetworkQueryTransformer[
  
     Query <: PipelineQuery with HasQualityFactorStatus with HasDeviceContext
  
   ](
  
     override val candidatePipelineIdentifier: CandidatePipelineIdentifier,
  
  -  override val maxTweetsToFetch: Int = MaxTweetsToFetch,
  
  -  override val sinceDuration: Duration = SinceDuration)
  
  +  override val maxTweetsToFetch: Int = MaxTweetsToFetch)
  
       extends CandidatePipelineQueryTransformer[Query, t.RecapQuery]
  
       with TimelineRankerQueryTransformer[Query] {
  
   
  
     override val candidateTweetSourceId = CandidateTweetSourceId.RecycledTweet
  
  -  override val skipVeryRecentTweets = false
  
     override val options = tweetKindOptions
  
   
  
  -  override def transform(input: Query): t.RecapQuery =
  
  -    buildTimelineRankerQuery(input).toThriftRecapQuery
  
  +  override def getTensorflowModel(query: Query): Option[String] = {
  
  +    Some(query.params(ScoredTweetsParam.EarlybirdTensorflowModel.InNetworkParam))
  
  +  }
  
  +
  
  +  override def transform(input: Query): t.RecapQuery = {
  
  +    val userState = input.features.get.getOrElse(UserStateFeature, None)
  
  +
  
  +    val sinceDuration =
  
  +      if (userState.exists(UserStatesForExtendedSinceDuration.contains)) ExpandedSinceDuration
  
  +      else DefaultSinceDuration
  
  +
  
  +    buildTimelineRankerQuery(input, sinceDuration).toThriftRecapQuery
  
  +  }
  
   }
  
