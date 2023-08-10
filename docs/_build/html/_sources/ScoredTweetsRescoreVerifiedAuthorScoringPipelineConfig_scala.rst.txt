a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/scoring_pipeline/ScoredTweetsRescoreVerifiedAuthorScoringPipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/scoring_pipeline/ScoredTweetsRescoreVerifiedAuthorScoringPipelineConfig.scala
==================================================

Change Range: -1,24 +0,0

Content:

::

deleted file mode 100644
  
  index d07e5c965..000000000
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/scoring_pipeline/ScoredTweetsRescoreVerifiedAuthorScoringPipelineConfig.scala
  
  +++ /dev/null
  
  -package com.twitter.home_mixer.product.scored_tweets.scoring_pipeline
  
  -
  
  -import com.twitter.home_mixer.functional_component.scorer.VerifiedAuthorScalingScorer
  
  -import com.twitter.home_mixer.product.scored_tweets.model.ScoredTweetsQuery
  
  -import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
  -import com.twitter.product_mixer.component_library.selector.InsertAppendResults
  
  -import com.twitter.product_mixer.core.functional_component.common.AllPipelines
  
  -import com.twitter.product_mixer.core.functional_component.scorer.Scorer
  
  -import com.twitter.product_mixer.core.functional_component.selector.Selector
  
  -import com.twitter.product_mixer.core.model.common.identifier.ScoringPipelineIdentifier
  
  -import com.twitter.product_mixer.core.pipeline.scoring.ScoringPipelineConfig
  
  -
  
  -object ScoredTweetsRescoreVerifiedAuthorScoringPipelineConfig
  
  -    extends ScoringPipelineConfig[ScoredTweetsQuery, TweetCandidate] {
  
  -
  
  -  override val identifier: ScoringPipelineIdentifier =
  
  -    ScoringPipelineIdentifier("ScoredTweetsRescoreVerifiedAuthor")
  
  -
  
  -  override val selectors: Seq[Selector[ScoredTweetsQuery]] =
  
  -    Seq(InsertAppendResults(AllPipelines))
  
  -
  
  -  override val scorers: Seq[Scorer[ScoredTweetsQuery, TweetCandidate]] =
  
  -    Seq(VerifiedAuthorScalingScorer)
  
  -}
  
