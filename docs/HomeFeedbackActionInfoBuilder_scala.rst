a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/HomeFeedbackActionInfoBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/HomeFeedbackActionInfoBuilder.scala
==================================================

Change Range: -12,7 +12,6

Content:

::

similarity index 96%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/HomeFeedbackActionInfoBuilder.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/HomeFeedbackActionInfoBuilder.scala
  
  index 032874ead..c932f6362 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/HomeFeedbackActionInfoBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/HomeFeedbackActionInfoBuilder.scala
  
  -package com.twitter.home_mixer.functional_component.decorator
  
  +package com.twitter.home_mixer.functional_component.decorator.urt.builder
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.SuggestTypeFeature
  
   import com.twitter.home_mixer.model.request.FollowingProduct
  
   import com.twitter.product_mixer.core.pipeline.PipelineQuery
  
   import com.twitter.timelines.service.{thriftscala => t}
  
   import com.twitter.timelines.util.FeedbackMetadataSerializer
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
