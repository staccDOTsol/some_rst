a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/BaseRealTimeAggregateQueryFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/BaseRealTimeAggregateQueryFeatureHydrator.scala
==================================================

Change Range: -24,7 +25,7

Content:

::

similarity index 84%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/BaseRealTimeAggregateQueryFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/BaseRealTimeAggregateQueryFeatureHydrator.scala
  
  index 772e0bb92..8f5b17d64 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/real_time_aggregates/BaseRealTimeAggregateQueryFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/real_time_aggregates/BaseRealTimeAggregateQueryFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator.real_time_aggregates
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator.real_time_aggregates
  
   
  
   import com.twitter.product_mixer.core.feature.Feature
  
   import com.twitter.product_mixer.core.feature.datarecord.DataRecordInAFeature
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMapBuilder
  
   import com.twitter.product_mixer.core.functional_component.feature_hydrator.QueryFeatureHydrator
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
  +import com.twitter.product_mixer.core.util.OffloadFuturePools
  
   import com.twitter.stitch.Stitch
  
   
  
   trait BaseRealTimeAggregateQueryFeatureHydrator[K]
  
   
  
     override def hydrate(
  
       query: PipelineQuery
  
  -  ): Stitch[FeatureMap] = {
  
  +  ): Stitch[FeatureMap] = OffloadFuturePools.offloadFuture {
  
       val possiblyKeys = keysFromQueryAndCandidates(query)
  
       fetchAndConstructDataRecords(Seq(possiblyKeys)).map { dataRecords =>
  
         FeatureMapBuilder()
  
