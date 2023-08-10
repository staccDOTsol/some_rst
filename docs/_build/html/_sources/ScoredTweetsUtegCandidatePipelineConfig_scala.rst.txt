a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsUtegCandidatePipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsUtegCandidatePipelineConfig.scala
==================================================

Change Range: -38,7 +37,7

Content:

::

index b58ca784f..9178d0789 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsUtegCandidatePipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/ScoredTweetsUtegCandidatePipelineConfig.scala
  
   package com.twitter.home_mixer.product.scored_tweets.candidate_pipeline
  
   
  
  -import com.twitter.home_mixer.functional_component.gate.MinCachedTweetsGate
  
  +import com.twitter.home_mixer.product.scored_tweets.gate.MinCachedTweetsGate
  
   import com.twitter.home_mixer.product.scored_tweets.model.ScoredTweetsQuery
  
   import com.twitter.home_mixer.product.scored_tweets.param.ScoredTweetsParam.CachedScoredTweets
  
  -import com.twitter.home_mixer.product.scored_tweets.param.ScoredTweetsParam.UtegSource
  
  +import com.twitter.home_mixer.product.scored_tweets.param.ScoredTweetsParam.CandidatePipeline
  
   import com.twitter.home_mixer.product.scored_tweets.query_transformer.TimelineRankerUtegQueryTransformer
  
   import com.twitter.home_mixer.product.scored_tweets.response_transformer.ScoredTweetsUtegResponseFeatureTransformer
  
   import com.twitter.product_mixer.component_library.candidate_source.timeline_ranker.TimelineRankerUtegCandidateSource
  
   import com.twitter.product_mixer.core.pipeline.candidate.CandidatePipelineConfig
  
   import com.twitter.timelineranker.{thriftscala => t}
  
   import com.twitter.timelines.configapi.decider.DeciderParam
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
       CandidatePipelineIdentifier("ScoredTweetsUteg")
  
   
  
     override val enabledDeciderParam: Option[DeciderParam[Boolean]] =
  
  -    Some(UtegSource.EnableCandidatePipelineParam)
  
  +    Some(CandidatePipeline.EnableUtegParam)
  
   
  
     override val gates: Seq[Gate[ScoredTweetsQuery]] = Seq(
  
       MinCachedTweetsGate(identifier, CachedScoredTweets.MinCachedTweetsParam)
  
