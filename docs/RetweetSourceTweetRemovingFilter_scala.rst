a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/RetweetSourceTweetRemovingFilter.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/filter/RetweetSourceTweetRemovingFilter.scala
==================================================

Change Range: -1,4 +1,4

Content:

::

similarity index 96%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/RetweetSourceTweetRemovingFilter.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/filter/RetweetSourceTweetRemovingFilter.scala
  
  index 6dc88f02c..5900756d3 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/RetweetSourceTweetRemovingFilter.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/filter/RetweetSourceTweetRemovingFilter.scala
  
  -package com.twitter.home_mixer.functional_component.filter
  
  +package com.twitter.home_mixer.product.scored_tweets.filter
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.EarlybirdFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.InReplyToTweetIdFeature
  
