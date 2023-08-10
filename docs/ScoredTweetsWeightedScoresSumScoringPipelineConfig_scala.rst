a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/scoring_pipeline/ScoredTweetsWeightedScoresSumScoringPipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/scoring_pipeline/ScoredTweetsWeightedScoresSumScoringPipelineConfig.scala
==================================================

Change Range: -1,34 +0,0

Content:

::

deleted file mode 100644
  
  index 459bc4d44..000000000
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/scoring_pipeline/ScoredTweetsWeightedScoresSumScoringPipelineConfig.scala
  
  +++ /dev/null
  
  -package com.twitter.home_mixer.product.scored_tweets.scoring_pipeline
  
  -
  
  -import com.twitter.home_mixer.product.scored_tweets.candidate_pipeline.CachedScoredTweetsCandidatePipelineConfig
  
  -import com.twitter.home_mixer.product.scored_tweets.model.ScoredTweetsQuery
  
  -import com.twitter.home_mixer.product.scored_tweets.scorer.WeightedScoresSumScorer
  
  -import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
  -import com.twitter.product_mixer.component_library.selector.InsertAppendResults
  
  -import com.twitter.product_mixer.core.functional_component.common.AllExceptPipelines
  
  -import com.twitter.product_mixer.core.functional_component.scorer.Scorer
  
  -import com.twitter.product_mixer.core.functional_component.selector.Selector
  
  -import com.twitter.product_mixer.core.model.common.identifier.ScoringPipelineIdentifier
  
  -import com.twitter.product_mixer.core.pipeline.scoring.ScoringPipelineConfig
  
  -import javax.inject.Inject
  
  -import javax.inject.Singleton
  
  -
  
  -@Singleton
  
  -class ScoredTweetsWeightedScoresSumScoringPipelineConfig @Inject() (
  
  -  weightedScoresSumScorer: WeightedScoresSumScorer)
  
  -    extends ScoringPipelineConfig[ScoredTweetsQuery, TweetCandidate] {
  
  -
  
  -  override val identifier: ScoringPipelineIdentifier =
  
  -    ScoringPipelineIdentifier("ScoredTweetsWeightedScoresSum")
  
  -
  
  -  override val selectors: Seq[Selector[ScoredTweetsQuery]] = Seq(
  
  -    InsertAppendResults(
  
  -      AllExceptPipelines(pipelinesToExclude =
  
  -        Set(CachedScoredTweetsCandidatePipelineConfig.Identifier))
  
  -    )
  
  -  )
  
  -
  
  -  override val scorers: Seq[Scorer[ScoredTweetsQuery, TweetCandidate]] = Seq(
  
  -    weightedScoresSumScorer
  
  -  )
  
  -}
  
