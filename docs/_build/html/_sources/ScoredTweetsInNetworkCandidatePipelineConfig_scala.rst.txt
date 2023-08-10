a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsInNetworkCandidatePipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsInNetworkCandidatePipelineConfig.scala
==================================================

Change Range: -69,7 +69,7

Content:

::

index 28118238d..63bd1add8 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsInNetworkCandidatePipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsInNetworkCandidatePipelineConfig.scala
  
   package com.twitter.home_mixer.product.scored_tweets.candidate_pipeline
  
   
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.ReplyFeatureHydrator
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.RetweetSourceTweetFeatureHydrator
  
  -import com.twitter.home_mixer.functional_component.filter.RetweetSourceTweetRemovingFilter
  
  -import com.twitter.home_mixer.functional_component.gate.MinCachedTweetsGate
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.IsExtendedReplyFeatureHydrator
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.ReplyFeatureHydrator
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.RetweetSourceTweetFeatureHydrator
  
  +import com.twitter.home_mixer.product.scored_tweets.filter.RetweetSourceTweetRemovingFilter
  
  +import com.twitter.home_mixer.product.scored_tweets.gate.MinCachedTweetsGate
  
   import com.twitter.home_mixer.product.scored_tweets.model.ScoredTweetsQuery
  
   import com.twitter.home_mixer.product.scored_tweets.param.ScoredTweetsParam.CachedScoredTweets
  
  -import com.twitter.home_mixer.product.scored_tweets.param.ScoredTweetsParam.InNetworkSource
  
  +import com.twitter.home_mixer.product.scored_tweets.param.ScoredTweetsParam.CandidatePipeline
  
   import com.twitter.home_mixer.product.scored_tweets.query_transformer.TimelineRankerInNetworkQueryTransformer
  
   import com.twitter.home_mixer.product.scored_tweets.response_transformer.ScoredTweetsInNetworkResponseFeatureTransformer
  
   import com.twitter.product_mixer.component_library.candidate_source.timeline_ranker.TimelineRankerInNetworkCandidateSource
  
   import com.twitter.product_mixer.core.pipeline.candidate.CandidatePipelineConfig
  
   import com.twitter.timelineranker.{thriftscala => t}
  
   import com.twitter.timelines.configapi.decider.DeciderParam
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
       CandidatePipelineIdentifier("ScoredTweetsInNetwork")
  
   
  
     override val enabledDeciderParam: Option[DeciderParam[Boolean]] =
  
  -    Some(InNetworkSource.EnableCandidatePipelineParam)
  
  +    Some(CandidatePipeline.EnableInNetworkParam)
  
   
  
     override val gates: Seq[Gate[ScoredTweetsQuery]] = Seq(
  
       MinCachedTweetsGate(identifier, CachedScoredTweets.MinCachedTweetsParam)
  
   
  
     override val postFilterFeatureHydration: Seq[
  
       BaseCandidateFeatureHydrator[PipelineQuery, TweetCandidate, _]
  
  -  ] = Seq(replyFeatureHydrator)
  
  +  ] = Seq(IsExtendedReplyFeatureHydrator, replyFeatureHydrator)
  
   
  
     override val featuresFromCandidateSourceTransformers: Seq[
  
       CandidateFeatureTransformer[t.CandidateTweet]
  
