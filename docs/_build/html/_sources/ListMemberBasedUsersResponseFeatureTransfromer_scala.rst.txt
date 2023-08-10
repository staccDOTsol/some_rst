a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/ListMemberBasedUsersResponseFeatureTransfromer.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/ListMemberBasedUsersResponseFeatureTransfromer.scala
==================================================

Change Range: -1,7 +1,7

Content:

::

index bce2ed457..7153d1f06 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/ListMemberBasedUsersResponseFeatureTransfromer.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/ListMemberBasedUsersResponseFeatureTransfromer.scala
  
   package com.twitter.home_mixer.product.list_recommended_users
  
   
  
   import com.twitter.hermit.candidate.{thriftscala => t}
  
  -import com.twitter.home_mixer.product.list_recommended_users.model.ListFeatures.ScoreFeature
  
  +import com.twitter.home_mixer.product.list_recommended_users.model.ListRecommendedUsersFeatures.ScoreFeature
  
   import com.twitter.product_mixer.core.feature.Feature
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMapBuilder
  
