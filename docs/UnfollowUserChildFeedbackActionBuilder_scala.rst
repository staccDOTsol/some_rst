a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/UnfollowUserChildFeedbackActionBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/UnfollowUserChildFeedbackActionBuilder.scala
==================================================

Change Range: -11,7 +11,6

Content:

::

similarity index 97%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/UnfollowUserChildFeedbackActionBuilder.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/UnfollowUserChildFeedbackActionBuilder.scala
  
  index 8bb700b44..864ee06d9 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/UnfollowUserChildFeedbackActionBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/UnfollowUserChildFeedbackActionBuilder.scala
  
  -package com.twitter.home_mixer.functional_component.decorator
  
  +package com.twitter.home_mixer.functional_component.decorator.urt.builder
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.AuthorIdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.InNetworkFeature
  
   import com.twitter.product_mixer.core.model.marshalling.response.urt.metadata.RichFeedbackBehaviorToggleFollowUser
  
   import com.twitter.product_mixer.core.product.guice.scope.ProductScoped
  
   import com.twitter.stringcenter.client.StringCenter
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
