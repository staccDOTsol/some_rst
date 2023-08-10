a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/NotRelevantChildFeedbackActionBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/NotRelevantChildFeedbackActionBuilder.scala
==================================================

Change Range: -12,7 +12,6

Content:

::

similarity index 96%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/NotRelevantChildFeedbackActionBuilder.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/NotRelevantChildFeedbackActionBuilder.scala
  
  index 1e5536e0b..39df781ce 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/NotRelevantChildFeedbackActionBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/NotRelevantChildFeedbackActionBuilder.scala
  
  -package com.twitter.home_mixer.functional_component.decorator
  
  +package com.twitter.home_mixer.functional_component.decorator.urt.builder
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.SuggestTypeFeature
  
   import com.twitter.home_mixer.product.following.model.HomeMixerExternalStrings
  
   import com.twitter.timelineservice.model.FeedbackInfo
  
   import com.twitter.timelineservice.model.FeedbackMetadata
  
   import com.twitter.timelineservice.{thriftscala => tlst}
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
