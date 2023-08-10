a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/CachedScoredTweetsQueryFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/CachedScoredTweetsQueryFeatureHydrator.scala
==================================================

Change Range: -39,7 +38,7

Content:

::

index 7801a9645..873f043be 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/CachedScoredTweetsQueryFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/CachedScoredTweetsQueryFeatureHydrator.scala
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.servo.cache.TtlCache
  
   import com.twitter.stitch.Stitch
  
  -import com.twitter.timelines.model.UserId
  
   import com.twitter.util.Return
  
   import com.twitter.util.Throw
  
   import com.twitter.util.Time
  
   import javax.inject.Singleton
  
   
  
   /**
  
  - * Fetch scored Tweets from cache and exclude the seen ones
  
  + * Fetch scored Tweets from cache and exclude the expired ones
  
    */
  
   @Singleton
  
   case class CachedScoredTweetsQueryFeatureHydrator @Inject() (
  
  -  scoredTweetsCache: TtlCache[UserId, hmt.CachedScoredTweets])
  
  +  scoredTweetsCache: TtlCache[Long, hmt.ScoredTweetsResponse])
  
       extends QueryFeatureHydrator[PipelineQuery] {
  
   
  
     override val identifier: FeatureHydratorIdentifier =
  
       Stitch.callFuture(scoredTweetsCache.get(Seq(userId))).map { keyValueResult =>
  
         keyValueResult(userId) match {
  
           case Return(cachedCandidatesOpt) =>
  
  -          val cachedScoredTweets = cachedCandidatesOpt.map(_.tweets).getOrElse(Seq.empty)
  
  +          val cachedScoredTweets = cachedCandidatesOpt.map(_.scoredTweets).getOrElse(Seq.empty)
  
             val nonExpiredTweets = cachedScoredTweets.filter { tweet =>
  
               tweet.lastScoredTimestampMs.exists(Time.fromMilliseconds(_).untilNow < tweetScoreTtl)
  
             }
  
