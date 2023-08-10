a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/UserStateQueryFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/UserStateQueryFeatureHydrator.scala
==================================================

Change Range: -9,13 +9,12

Content:

::

similarity index 96%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/UserStateQueryFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/UserStateQueryFeatureHydrator.scala
  
  index 29532c8e1..5a61f31fb 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/UserStateQueryFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/feature_hydrator/UserStateQueryFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.scored_tweets.feature_hydrator
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.UserStateFeature
  
   import com.twitter.home_mixer.service.HomeMixerAlertConfig
  
   import com.twitter.product_mixer.core.model.common.identifier.FeatureHydratorIdentifier
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.stitch.Stitch
  
  -import com.twitter.timelines.user_health.{thriftscala => uh}
  
   import com.twitter.timelines.user_health.v1.{thriftscala => uhv1}
  
  +import com.twitter.timelines.user_health.{thriftscala => uh}
  
   import com.twitter.user_session_store.ReadOnlyUserSessionStore
  
   import com.twitter.user_session_store.ReadRequest
  
   import com.twitter.user_session_store.UserSessionDataset
  
   import com.twitter.user_session_store.UserSessionDataset.UserSessionDataset
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
