a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/model/ListFeatures.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/model/ListRecommendedUsersFeatures.scala
==================================================

Change Range: -1,12 +1,12

Content:

::

similarity index 65%
  
  rename from home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/model/ListFeatures.scala
  
  rename to home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/model/ListRecommendedUsersFeatures.scala
  
  index 9abcd6fb2..baed8171b 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/model/ListFeatures.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/model/ListRecommendedUsersFeatures.scala
  
   package com.twitter.home_mixer.product.list_recommended_users.model
  
   
  
  -import com.twitter.gizmoduck.{thriftscala => gt}
  
   import com.twitter.product_mixer.component_library.model.candidate.UserCandidate
  
   import com.twitter.product_mixer.core.feature.Feature
  
   
  
  -object ListFeatures {
  
  +object ListRecommendedUsersFeatures {
  
     // Candidate features
  
  -  object GizmoduckUserFeature extends Feature[UserCandidate, Option[gt.User]]
  
  +  object IsGizmoduckValidUserFeature extends Feature[UserCandidate, Boolean]
  
     object IsListMemberFeature extends Feature[UserCandidate, Boolean]
  
  +  object IsSGSValidUserFeature extends Feature[UserCandidate, Boolean]
  
     object ScoreFeature extends Feature[UserCandidate, Double]
  
   }
  
