a/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/CachedScoredTweetsHelper.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/CachedScoredTweetsHelper.scala
==================================================

Change Range: -39,7 +40,7

Content:

::

index 740fd03ec..e0fbdd76f 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/CachedScoredTweetsHelper.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/util/CachedScoredTweetsHelper.scala
  
       untilTime: Time
  
     ): Seq[Long] =
  
       tweetImpressionsAndCachedScoredTweets(features, candidatePipelineIdentifier)
  
  +      .filter { tweetId => SnowflakeId.isSnowflakeId(tweetId) }
  
         .filter { tweetId =>
  
           val creationTime = SnowflakeId.timeFromId(tweetId)
  
           sinceTime <= creationTime && untilTime >= creationTime
  
   
  
     def unseenCachedScoredTweets(
  
       features: FeatureMap
  
  -  ): Seq[hmt.CachedScoredTweet] = {
  
  +  ): Seq[hmt.ScoredTweet] = {
  
       val seenTweetIds = TweetImpressionsHelper.tweetImpressions(features)
  
   
  
       features
  
