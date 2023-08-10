a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TimelineServiceTweetsQueryFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/feature_hydrator/TimelineServiceTweetsQueryFeatureHydrator.scala
==================================================

Change Range: -16,8 +17,6

Content:

::

similarity index 94%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TimelineServiceTweetsQueryFeatureHydrator.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/feature_hydrator/TimelineServiceTweetsQueryFeatureHydrator.scala
  
  index fc9727cb7..e0ae2207e 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/feature_hydrator/TimelineServiceTweetsQueryFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/for_you/feature_hydrator/TimelineServiceTweetsQueryFeatureHydrator.scala
  
  -package com.twitter.home_mixer.functional_component.feature_hydrator
  
  +package com.twitter.home_mixer.product.for_you.feature_hydrator
  
   
  
   import com.twitter.home_mixer.marshaller.timelines.DeviceContextMarshaller
  
  +import com.twitter.home_mixer.model.HomeFeatures.TimelineServiceTweetsFeature
  
   import com.twitter.home_mixer.model.request.DeviceContext
  
   import com.twitter.home_mixer.model.request.HasDeviceContext
  
   import com.twitter.home_mixer.service.HomeMixerAlertConfig
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
  -object TimelineServiceTweetsFeature extends Feature[PipelineQuery, Seq[Long]]
  
  -
  
   @Singleton
  
   case class TimelineServiceTweetsQueryFeatureHydrator @Inject() (
  
     timelineService: TimelineService,
  
