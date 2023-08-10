a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/DontLikeFeedbackActionBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/DontLikeFeedbackActionBuilder.scala
==================================================

Change Range: -17,7 +17,6

Content:

::

similarity index 98%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/DontLikeFeedbackActionBuilder.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/DontLikeFeedbackActionBuilder.scala
  
  index 344ce668d..9032b53d9 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/DontLikeFeedbackActionBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/DontLikeFeedbackActionBuilder.scala
  
  -package com.twitter.home_mixer.functional_component.decorator
  
  +package com.twitter.home_mixer.functional_component.decorator.urt.builder
  
   
  
   import com.twitter.conversions.DurationOps._
  
   import com.twitter.home_mixer.model.HomeFeatures.SuggestTypeFeature
  
   import com.twitter.timelineservice.model.FeedbackInfo
  
   import com.twitter.timelineservice.model.FeedbackMetadata
  
   import com.twitter.timelineservice.{thriftscala => tls}
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
