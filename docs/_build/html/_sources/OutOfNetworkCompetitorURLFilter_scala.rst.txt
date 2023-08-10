a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/OutOfNetworkCompetitorURLFilter.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/filter/OutOfNetworkCompetitorURLFilter.scala
==================================================

Change Range: -32,6 +32,7

Content:

::

similarity index 90%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/OutOfNetworkCompetitorURLFilter.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/filter/OutOfNetworkCompetitorURLFilter.scala
  
  index f7163bee3..00e2bb200 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/OutOfNetworkCompetitorURLFilter.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/scored_tweets/filter/OutOfNetworkCompetitorURLFilter.scala
  
  -package com.twitter.home_mixer.functional_component.filter
  
  +package com.twitter.home_mixer.product.scored_tweets.filter
  
   
  
   import com.twitter.home_mixer.model.HomeFeatures.InNetworkFeature
  
   import com.twitter.home_mixer.model.HomeFeatures.IsRetweetFeature
  
     ): Boolean = {
  
       !candidate.features.getOrElse(InNetworkFeature, true) &&
  
       !candidate.features.getOrElse(IsRetweetFeature, false) &&
  
  -    candidate.features.get(TweetUrlsFeature).toSet.intersect(competitorUrls).nonEmpty
  
  +    candidate.features
  
  +      .getOrElse(TweetUrlsFeature, Seq.empty).toSet.intersect(competitorUrls).nonEmpty
  
     }
  
   }
  
