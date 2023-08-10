a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RealTimeInteractionGraphUserVertexQueryFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/RealTimeInteractionGraphUserVertexQueryFeatureHydrator.scala
==================================================

Change Range: -13,7 +13,6

Content:

::

similarity index 96%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RealTimeInteractionGraphUserVertexQueryFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/RealTimeInteractionGraphUserVertexQueryFeatureHydrator.scala
  
  index 899f07630..60d82cc28 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/RealTimeInteractionGraphUserVertexQueryFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/RealTimeInteractionGraphUserVertexQueryFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
   import com.google.inject.name.Named
  
   import com.twitter.finagle.stats.StatsReceiver
  
   import com.twitter.servo.cache.ReadCache
  
   import com.twitter.stitch.Stitch
  
   import com.twitter.wtf.real_time_interaction_graph.{thriftscala => ig}
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
