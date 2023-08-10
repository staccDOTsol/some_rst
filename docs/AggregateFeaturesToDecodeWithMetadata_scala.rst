a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/offline_aggregates/AggregateFeaturesToDecodeWithMetadata.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/offline_aggregates/AggregateFeaturesToDecodeWithMetadata.scala
==================================================

Change Range: -1,4 +1,4

Content:

::

similarity index 96%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/offline_aggregates/AggregateFeaturesToDecodeWithMetadata.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/offline_aggregates/AggregateFeaturesToDecodeWithMetadata.scala
  
  index cbda35437..5fb599240 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/offline_aggregates/AggregateFeaturesToDecodeWithMetadata.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/offline_aggregates/AggregateFeaturesToDecodeWithMetadata.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator.offline_aggregates
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator.offline_aggregates
  
   
  
   import com.twitter.finagle.stats.NullStatsReceiver
  
   import com.twitter.timelinemixer.injection.repository.uss.VersionedAggregateFeaturesDecoder
  
