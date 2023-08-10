a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/query_transformer/EditedTweetsCandidatePipelineQueryTransformer.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/query_transformer/EditedTweetsCandidatePipelineQueryTransformer.scala
==================================================

Change Range: -29,8 +29,8

Content:

::

index 731f7166e..8753f2f28 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/query_transformer/EditedTweetsCandidatePipelineQueryTransformer.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/query_transformer/EditedTweetsCandidatePipelineQueryTransformer.scala
  
     override val identifier: TransformerIdentifier = TransformerIdentifier("EditedTweets")
  
   
  
     // The time window for which a tweet remains editable after creation.
  
  -  private val EditTimeWindow = 30.minutes
  
  +  private val EditTimeWindow = 60.minutes
  
   
  
     override def transform(query: PipelineQuery): Seq[Long] = {
  
       val applicableCandidates = getApplicableCandidates(query)
  
         // Any tweets in it could have become stale since being served.
  
         val previousTimelineLoadTime = applicableCandidates.head.servedTime
  
   
  
  -      // The time window for editing a tweet is 30 minutes,
  
  -      // so we ignore responses older than (PTL Time - 30 mins).
  
  +      // The time window for editing a tweet is 60 minutes,
  
  +      // so we ignore responses older than (PTL Time - 60 mins).
  
         val inWindowCandidates: Seq[PersistenceStoreEntry] = applicableCandidates
  
           .takeWhile(_.servedTime.until(previousTimelineLoadTime) < EditTimeWindow)
  
   
  
