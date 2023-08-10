a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/FeedbackFatigueFilter.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/FeedbackFatigueFilter.scala
==================================================

Change Range: -72,7 +72,7

Content:

::

index 5b6541f51..582583e7f 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/FeedbackFatigueFilter.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/functional_component/filter/FeedbackFatigueFilter.scala
  
   
  
         originalAuthorId.exists(authorsToFilter.contains) ||
  
         (likers.nonEmpty && eligibleLikers.isEmpty) ||
  
  -      (followers.nonEmpty && eligibleFollowers.isEmpty) ||
  
  +      (followers.nonEmpty && eligibleFollowers.isEmpty && likers.isEmpty) ||
  
         (candidate.features.getOrElse(IsRetweetFeature, false) &&
  
         authorId.exists(retweetersToFilter.contains))
  
       }
  
