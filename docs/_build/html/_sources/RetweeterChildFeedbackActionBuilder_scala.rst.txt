a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/RetweeterChildFeedbackActionBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/RetweeterChildFeedbackActionBuilder.scala
==================================================

Change Range: -10,7 +10,6

Content:

::

similarity index 95%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/RetweeterChildFeedbackActionBuilder.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/RetweeterChildFeedbackActionBuilder.scala
  
  index f2ec8d6f5..006e93b58 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/RetweeterChildFeedbackActionBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/RetweeterChildFeedbackActionBuilder.scala
  
  -package com.twitter.home_mixer.functional_component.decorator
  
  +package com.twitter.home_mixer.functional_component.decorator.urt.builder
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.IsRetweetFeature
  
   import com.twitter.product_mixer.core.product.guice.scope.ProductScoped
  
   import com.twitter.stringcenter.client.StringCenter
  
   import com.twitter.timelines.service.{thriftscala => t}
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
