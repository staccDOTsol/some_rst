a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/ScoredTweetsCrMixerResponseFeatureTransformer.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/ScoredTweetsPopularVideosResponseFeatureTransformer.scala
==================================================

Change Range: -15,51 +15,32

Content:

::

similarity index 51%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/ScoredTweetsCrMixerResponseFeatureTransformer.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/ScoredTweetsPopularVideosResponseFeatureTransformer.scala
  
  index 96aef36b6..6657e6f5c 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/ScoredTweetsCrMixerResponseFeatureTransformer.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/response_transformer/ScoredTweetsPopularVideosResponseFeatureTransformer.scala
  
   package com.twitter.home_mixer.product.scored_tweets.response_transformer
  
   
  
  -import com.twitter.cr_mixer.{thriftscala => crm}
  
  +import com.twitter.explore_ranker.{thriftscala => ert}
  
   import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.CandidateSourceIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.FromInNetworkSourceFeature
  
  +import com.twitter.home_mixer.model.HomeFeatures.HasVideoFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.IsRandomTweetFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.StreamToKafkaFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SuggestTypeFeature
  
  -import com.twitter.home_mixer.model.HomeFeatures.TSPMetricTagFeature
  
   import com.twitter.product_mixer.core.feature.Feature
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMapBuilder
  
   import com.twitter.product_mixer.core.model.common.identifier.TransformerIdentifier
  
   import com.twitter.timelineservice.suggests.logging.candidate_tweet_source_id.{thriftscala => cts}
  
   import com.twitter.timelineservice.suggests.{thriftscala => st}
  
  -import com.twitter.tsp.{thriftscala => tsp}
  
   
  
  -object ScoredTweetsCrMixerResponseFeatureTransformer
  
  -    extends CandidateFeatureTransformer[crm.TweetRecommendation] {
  
  +object ScoredTweetsPopularVideosResponseFeatureTransformer
  
  +    extends CandidateFeatureTransformer[ert.ExploreTweetRecommendation] {
  
   
  
     override val identifier: TransformerIdentifier =
  
  -    TransformerIdentifier("ScoredTweetsCrMixerResponse")
  
  +    TransformerIdentifier("ScoredTweetsPopularVideosResponse")
  
   
  
     override val features: Set[Feature[_, _]] = Set(
  
       AuthorIdFeature,
  
       CandidateSourceIdFeature,
  
       FromInNetworkSourceFeature,
  
  +    HasVideoFeature,
  
       IsRandomTweetFeature,
  
       StreamToKafkaFeature,
  
  -    SuggestTypeFeature,
  
  -    TSPMetricTagFeature
  
  +    SuggestTypeFeature
  
     )
  
   
  
  -  override def transform(candidate: crm.TweetRecommendation): FeatureMap = {
  
  -    val crMixerMetricTags = candidate.metricTags.getOrElse(Seq.empty)
  
  -    val tspMetricTag = crMixerMetricTags
  
  -      .map(CrMixerMetricTagToTspMetricTag)
  
  -      .filter(_.nonEmpty).map(_.get).toSet
  
  -
  
  +  override def transform(candidate: ert.ExploreTweetRecommendation): FeatureMap = {
  
       FeatureMapBuilder()
  
         .add(AuthorIdFeature, candidate.authorId)
  
  -      .add(CandidateSourceIdFeature, Some(cts.CandidateTweetSourceId.Simcluster))
  
  +      .add(CandidateSourceIdFeature, Some(cts.CandidateTweetSourceId.MediaTweet))
  
         .add(FromInNetworkSourceFeature, false)
  
  +      .add(HasVideoFeature, candidate.mediaType.contains(ert.MediaType.Video))
  
         .add(IsRandomTweetFeature, false)
  
         .add(StreamToKafkaFeature, true)
  
  -      .add(SuggestTypeFeature, Some(st.SuggestType.ScTweet))
  
  -      .add(TSPMetricTagFeature, tspMetricTag)
  
  +      .add(SuggestTypeFeature, Some(st.SuggestType.MediaTweet))
  
         .build()
  
     }
  
  -
  
  -  private def CrMixerMetricTagToTspMetricTag(
  
  -    crMixerMetricTag: crm.MetricTag
  
  -  ): Option[tsp.MetricTag] = crMixerMetricTag match {
  
  -    case crm.MetricTag.TweetFavorite => Some(tsp.MetricTag.TweetFavorite)
  
  -    case crm.MetricTag.Retweet => Some(tsp.MetricTag.Retweet)
  
  -    case crm.MetricTag.UserFollow => Some(tsp.MetricTag.UserFollow)
  
  -    case crm.MetricTag.PushOpenOrNtabClick => Some(tsp.MetricTag.PushOpenOrNtabClick)
  
  -    case crm.MetricTag.UserInterestedIn => Some(tsp.MetricTag.UserInterestedIn)
  
  -    case crm.MetricTag.HomeTweetClick => Some(tsp.MetricTag.HomeTweetClick)
  
  -    case crm.MetricTag.HomeVideoView => Some(tsp.MetricTag.HomeVideoView)
  
  -    case _ => None
  
  -  }
  
   }
  
