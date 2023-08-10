a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/CachedScoredTweetsCandidatePipelineConfig.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/CachedScoredTweetsCandidatePipelineConfig.scala
==================================================

Change Range: -35,15 +35,15

Content:

::

index 95cc79aad..65a42cbee 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/CachedScoredTweetsCandidatePipelineConfig.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_pipeline/CachedScoredTweetsCandidatePipelineConfig.scala
  
       extends CandidatePipelineConfig[
  
         ScoredTweetsQuery,
  
         ScoredTweetsQuery,
  
  -      hmt.CachedScoredTweet,
  
  +      hmt.ScoredTweet,
  
         TweetCandidate
  
       ] {
  
   
  
       ScoredTweetsQuery
  
     ] = identity
  
   
  
  -  override val candidateSource: BaseCandidateSource[ScoredTweetsQuery, hmt.CachedScoredTweet] =
  
  +  override val candidateSource: BaseCandidateSource[ScoredTweetsQuery, hmt.ScoredTweet] =
  
       cachedScoredTweetsCandidateSource
  
   
  
     override val featuresFromCandidateSourceTransformers: Seq[
  
  -    CandidateFeatureTransformer[hmt.CachedScoredTweet]
  
  +    CandidateFeatureTransformer[hmt.ScoredTweet]
  
     ] = Seq(CachedScoredTweetsResponseFeatureTransformer)
  
   
  
     override val resultTransformer: CandidatePipelineResultsTransformer[
  
  -    hmt.CachedScoredTweet,
  
  +    hmt.ScoredTweet,
  
       TweetCandidate
  
     ] = { sourceResult => TweetCandidate(id = sourceResult.tweetId) }
  
   }
  
