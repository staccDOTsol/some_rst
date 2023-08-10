a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsFrsCandidatePipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsFrsCandidatePipelineConfig.scala
==================================================

Change Range: -40,6 +42,9

Content:

::

index 018eaa688..f25a839b7 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsFrsCandidatePipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsFrsCandidatePipelineConfig.scala
  
   package com.twitter.home_mixer.product.scored_tweets.candidate_pipeline
  
   
  
  -import com.twitter.home_mixer.functional_component.gate.MinCachedTweetsGate
  
  +import com.twitter.home_mixer.product.scored_tweets.feature_hydrator.FrsSeedUsersQueryFeatureHydrator
  
  +import com.twitter.home_mixer.product.scored_tweets.gate.MinCachedTweetsGate
  
   import com.twitter.home_mixer.product.scored_tweets.model.ScoredTweetsQuery
  
   import com.twitter.home_mixer.product.scored_tweets.param.ScoredTweetsParam.CachedScoredTweets
  
  -import com.twitter.home_mixer.product.scored_tweets.query_feature_hydrator.FrsSeedUsersQueryFeatureHydrator
  
  +import com.twitter.home_mixer.product.scored_tweets.param.ScoredTweetsParam.CandidatePipeline
  
   import com.twitter.home_mixer.product.scored_tweets.query_transformer.TimelineRankerFrsQueryTransformer
  
   import com.twitter.home_mixer.product.scored_tweets.response_transformer.ScoredTweetsFrsResponseFeatureTransformer
  
   import com.twitter.product_mixer.component_library.candidate_source.timeline_ranker.TimelineRankerRecapCandidateSource
  
   import com.twitter.product_mixer.core.model.common.identifier.CandidatePipelineIdentifier
  
   import com.twitter.product_mixer.core.pipeline.candidate.CandidatePipelineConfig
  
   import com.twitter.timelineranker.{thriftscala => tlr}
  
  +import com.twitter.timelines.configapi.decider.DeciderParam
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
     override val identifier: CandidatePipelineIdentifier =
  
       CandidatePipelineIdentifier("ScoredTweetsFrs")
  
   
  
  +  override val enabledDeciderParam: Option[DeciderParam[Boolean]] =
  
  +    Some(CandidatePipeline.EnableFrsParam)
  
  +
  
     override val gates: Seq[Gate[ScoredTweetsQuery]] = Seq(
  
       MinCachedTweetsGate(identifier, CachedScoredTweets.MinCachedTweetsParam)
  
     )
  
