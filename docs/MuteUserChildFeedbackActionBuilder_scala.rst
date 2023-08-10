a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/MuteUserChildFeedbackActionBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/MuteUserChildFeedbackActionBuilder.scala
==================================================

Change Range: -12,7 +12,6

Content:

::

similarity index 96%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/MuteUserChildFeedbackActionBuilder.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/MuteUserChildFeedbackActionBuilder.scala
  
  index 1c8d24ef1..542ed8a67 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/MuteUserChildFeedbackActionBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/MuteUserChildFeedbackActionBuilder.scala
  
  -package com.twitter.home_mixer.functional_component.decorator
  
  +package com.twitter.home_mixer.functional_component.decorator.urt.builder
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.IsRetweetFeature
  
   import com.twitter.product_mixer.core.model.marshalling.response.urt.metadata.RichFeedbackBehaviorToggleMuteUser
  
   import com.twitter.product_mixer.core.product.guice.scope.ProductScoped
  
   import com.twitter.stringcenter.client.StringCenter
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
