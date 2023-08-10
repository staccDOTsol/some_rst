a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/BlockUserChildFeedbackActionBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/BlockUserChildFeedbackActionBuilder.scala
==================================================

Change Range: -13,7 +13,6

Content:

::

similarity index 96%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/BlockUserChildFeedbackActionBuilder.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/BlockUserChildFeedbackActionBuilder.scala
  
  index 2535b24b2..a23b57dee 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/BlockUserChildFeedbackActionBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/BlockUserChildFeedbackActionBuilder.scala
  
  -package com.twitter.home_mixer.functional_component.decorator
  
  +package com.twitter.home_mixer.functional_component.decorator.urt.builder
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.IsRetweetFeature
  
   import com.twitter.product_mixer.core.model.marshalling.response.urt.metadata.RichFeedbackBehaviorBlockUser
  
   import com.twitter.product_mixer.core.product.guice.scope.ProductScoped
  
   import com.twitter.stringcenter.client.StringCenter
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
