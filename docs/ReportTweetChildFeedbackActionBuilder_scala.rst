a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/ReportTweetChildFeedbackActionBuilder.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/ReportTweetChildFeedbackActionBuilder.scala
==================================================

Change Range: -8,7 +8,6

Content:

::

similarity index 95%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/ReportTweetChildFeedbackActionBuilder.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/ReportTweetChildFeedbackActionBuilder.scala
  
  index 3938a476d..a4ef0cbb2 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/ReportTweetChildFeedbackActionBuilder.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/decorator/urt/builder/ReportTweetChildFeedbackActionBuilder.scala
  
  -package com.twitter.home_mixer.functional_component.decorator
  
  +package com.twitter.home_mixer.functional_component.decorator.urt.builder
  
   
  
   import com.twitter.home_mixer.product.following.model.HomeMixerExternalStrings
  
   import com.twitter.product_mixer.component_library.model.candidate.TweetCandidate
  
   import com.twitter.product_mixer.core.model.marshalling.response.urt.metadata.RichFeedbackBehaviorReportTweet
  
   import com.twitter.product_mixer.core.product.guice.scope.ProductScoped
  
   import com.twitter.stringcenter.client.StringCenter
  
  -
  
   import javax.inject.Inject
  
   import javax.inject.Singleton
  
   
  
