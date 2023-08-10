a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/AuthorChildFeedbackActionBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/AuthorChildFeedbackActionBuilder.scala
==================================================

Change Range: -9,7 +9,6

Content:

::

similarity index 95%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/AuthorChildFeedbackActionBuilder.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/AuthorChildFeedbackActionBuilder.scala
  
  index 712d85360..dee273f5f 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/AuthorChildFeedbackActionBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/AuthorChildFeedbackActionBuilder.scala
  
  -package com.twitter.home_mixer.functional_component.decorator
  
  +package com.twitter.home_mixer.functional_component.decorator.urt.builder
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.ScreenNamesFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.SuggestTypeFeature
  
   import com.twitter.product_mixer.core.product.guice.scope.ProductScoped
  
   import com.twitter.stringcenter.client.StringCenter
  
   import com.twitter.timelines.service.{thriftscala => t}
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
