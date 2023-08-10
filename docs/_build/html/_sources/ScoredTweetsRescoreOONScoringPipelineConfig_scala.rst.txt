a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/scoring_pipeline/ScoredTweetsRescoreOONScoringPipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/scoring_pipeline/ScoredTweetsHeuristicScoringPipelineConfig.scala
==================================================

Change Range: -10,14 +10,14

Content:

::

similarity index 73%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/scoring_pipeline/ScoredTweetsRescoreOONScoringPipelineConfig.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/scoring_pipeline/ScoredTweetsHeuristicScoringPipelineConfig.scala
  
  index 71d53d2ec..aedfc15b5 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/scoring_pipeline/ScoredTweetsRescoreOONScoringPipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/scoring_pipeline/ScoredTweetsHeuristicScoringPipelineConfig.scala
  
   package com.twitter.home_mixer.product.scored_tweets.scoring_pipeline
  
   
  
  -import com.twitter.home_mixer.functional_component.scorer.OONTweetScalingScorer
  
   import com.twitter.home_mixer.product.scored_tweets.model.ScoredTweetsQuery
  
  +import com.twitter.home_mixer.product.scored_tweets.scorer.HeuristicScorer
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.component_library.selector.InsertAppendResults
  
   import com.twitter.product_mixer.core.functional_component.common.AllPipelines
  
   import com.twitter.product_mixer.core.model.common.identifier.ScoringPipelineIdentifier
  
   import com.twitter.product_mixer.core.pipeline.scoring.ScoringPipelineConfig
  
   
  
  -object ScoredTweetsRescoreOONScoringPipelineConfig
  
  +object ScoredTweetsHeuristicScoringPipelineConfig
  
       extends ScoringPipelineConfig[ScoredTweetsQuery, TweetCandidate] {
  
   
  
     override val identifier: ScoringPipelineIdentifier =
  
  -    ScoringPipelineIdentifier("ScoredTweetsRescoreOON")
  
  +    ScoringPipelineIdentifier("ScoredTweetsHeuristic")
  
   
  
  -  override val selectors: Seq[Selector[ScoredTweetsQuery]] =
  
  -    Seq(InsertAppendResults(AllPipelines))
  
  +  override val selectors: Seq[Selector[ScoredTweetsQuery]] = Seq(InsertAppendResults(AllPipelines))
  
   
  
  -  override val scorers: Seq[Scorer[ScoredTweetsQuery, TweetCandidate]] = Seq(OONTweetScalingScorer)
  
  +  override val scorers: Seq[Scorer[ScoredTweetsQuery, TweetCandidate]] =
  
  +    Seq(HeuristicScorer)
  
   }
  
