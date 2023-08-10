a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/NotInterestedTopicFeedbackActionBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/NotInterestedTopicFeedbackActionBuilder.scala
==================================================

Change Range: -15,7 +15,6

Content:

::

similarity index 97%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/NotInterestedTopicFeedbackActionBuilder.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/NotInterestedTopicFeedbackActionBuilder.scala
  
  index 7c8aebb07..2ec520e11 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/NotInterestedTopicFeedbackActionBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/NotInterestedTopicFeedbackActionBuilder.scala
  
  -package com.twitter.home_mixer.functional_component.decorator
  
  +package com.twitter.home_mixer.functional_component.decorator.urt.builder
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.InNetworkFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.PerspectiveFilteredLikedByUserIdsFeature
  
   import com.twitter.product_mixer.core.model.marshalling.response.urt.metadata.RichFeedbackBehaviorMarkNotInterestedTopic
  
   import com.twitter.product_mixer.core.product.guice.scope.ProductScoped
  
   import com.twitter.stringcenter.client.StringCenter
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
