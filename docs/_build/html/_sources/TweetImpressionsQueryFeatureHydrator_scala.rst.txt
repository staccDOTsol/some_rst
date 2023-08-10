a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetImpressionsQueryFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetImpressionsQueryFeatureHydrator.scala
==================================================

Change Range: -24,8 +24,8

Content:

::

index 4e585ec45..0fc6fd1f5 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetImpressionsQueryFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TweetImpressionsQueryFeatureHydrator.scala
  
     manhattanTweetImpressionStoreClient: ManhattanTweetImpressionStoreClient)
  
       extends QueryFeatureHydrator[Query] {
  
   
  
  -  private val TweetImpressionTTL = 1.day
  
  -  private val TweetImpressionCap = 3000
  
  +  private val TweetImpressionTTL = 2.days
  
  +  private val TweetImpressionCap = 5000
  
   
  
     override val identifier: FeatureHydratorIdentifier = FeatureHydratorIdentifier("TweetImpressions")
  
   
  
