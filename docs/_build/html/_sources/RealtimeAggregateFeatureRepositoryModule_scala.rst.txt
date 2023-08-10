a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/RealtimeAggregateFeatureRepositoryModule.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/RealtimeAggregateFeatureRepositoryModule.scala
==================================================

Change Range: -208,42 +208,20

Content:

::

index 19512e663..c3c545819 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/RealtimeAggregateFeatureRepositoryModule.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/module/RealtimeAggregateFeatureRepositoryModule.scala
  
       extends TwitterModule
  
       with RealtimeAggregateHelpers {
  
   
  
  -  private val authorIdFeature = new Feature.Discrete("entities.source_author_id")
  
  -  private val countryCodeFeature = new Feature.Text("geo.user_location.country_code")
  
  -  private val listIdFeature = new Feature.Discrete("list.id")
  
  -  private val userIdFeature = new Feature.Discrete("meta.user_id")
  
  -  private val topicIdFeature = new Feature.Discrete("entities.topic_id")
  
  -  private val tweetIdFeature = new Feature.Discrete("entities.source_tweet_id")
  
  +  private val authorIdFeature = new Feature.Discrete("entities.source_author_id").getFeatureId
  
  +  private val countryCodeFeature = new Feature.Text("geo.user_location.country_code").getFeatureId
  
  +  private val listIdFeature = new Feature.Discrete("list.id").getFeatureId
  
  +  private val userIdFeature = new Feature.Discrete("meta.user_id").getFeatureId
  
  +  private val topicIdFeature = new Feature.Discrete("entities.topic_id").getFeatureId
  
  +  private val tweetIdFeature = new Feature.Discrete("entities.source_tweet_id").getFeatureId
  
   
  
     @Provides
  
     @Singleton
  
         .compose((k: AggregationKey) => (k, defaultBatchID))
  
     }
  
   
  
  -  protected def keyTransformD1(f1: Feature.Discrete)(key: Long): String = {
  
  -    val aggregationKey = AggregationKey(
  
  -      Map(f1.getFeatureId -> key),
  
  -      Map.empty
  
  -    )
  
  -
  
  +  protected def keyTransformD1(f1: Long)(key: Long): String = {
  
  +    val aggregationKey = AggregationKey(Map(f1 -> key), Map.empty)
  
       keyEncoder(aggregationKey)
  
     }
  
   
  
  -  protected def keyTransformD2(
  
  -    f1: Feature.Discrete,
  
  -    f2: Feature.Discrete
  
  -  )(
  
  -    keys: (Long, Long)
  
  -  ): String = {
  
  +  protected def keyTransformD2(f1: Long, f2: Long)(keys: (Long, Long)): String = {
  
       val (k1, k2) = keys
  
  -    val aggregationKey = AggregationKey(
  
  -      Map(f1.getFeatureId -> k1, f2.getFeatureId -> k2),
  
  -      Map.empty
  
  -    )
  
  -
  
  +    val aggregationKey = AggregationKey(Map(f1 -> k1, f2 -> k2), Map.empty)
  
       keyEncoder(aggregationKey)
  
     }
  
   
  
  -  protected def keyTransformD1T1(
  
  -    f1: Feature.Discrete,
  
  -    f2: Feature.Text
  
  -  )(
  
  -    keys: (Long, String)
  
  -  ): String = {
  
  +  protected def keyTransformD1T1(f1: Long, f2: Long)(keys: (Long, String)): String = {
  
       val (k1, k2) = keys
  
  -    val aggregationKey = AggregationKey(
  
  -      Map(f1.getFeatureId -> k1),
  
  -      Map(f2.getFeatureId -> k2)
  
  -    )
  
  -
  
  +    val aggregationKey = AggregationKey(Map(f1 -> k1), Map(f2 -> k2))
  
       keyEncoder(aggregationKey)
  
     }
  
   
  
