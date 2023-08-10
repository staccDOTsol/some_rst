a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_source/CachedScoredTweetsCandidateSource.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_source/CachedScoredTweetsCandidateSource.scala
==================================================

Change Range: -12,12 +12,12

Content:

::

index 15a522efa..a5f556e7c 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_source/CachedScoredTweetsCandidateSource.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/candidate_source/CachedScoredTweetsCandidateSource.scala
  
   
  
   @Singleton
  
   class CachedScoredTweetsCandidateSource @Inject() ()
  
  -    extends CandidateSource[PipelineQuery, hmt.CachedScoredTweet] {
  
  +    extends CandidateSource[PipelineQuery, hmt.ScoredTweet] {
  
   
  
     override val identifier: CandidateSourceIdentifier =
  
       CandidateSourceIdentifier("CachedScoredTweets")
  
   
  
  -  override def apply(request: PipelineQuery): Stitch[Seq[hmt.CachedScoredTweet]] = {
  
  +  override def apply(request: PipelineQuery): Stitch[Seq[hmt.ScoredTweet]] = {
  
       Stitch.value(
  
         request.features.map(CachedScoredTweetsHelper.unseenCachedScoredTweets).getOrElse(Seq.empty))
  
     }
  
