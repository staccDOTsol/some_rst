a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/feature_hydrator/IsListMemberFeatureHydrator.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/feature_hydrator/IsListMemberFeatureHydrator.scala
==================================================

Change Range: -1,7 +1,7

Content:

::

index a7c51b4b1..c9e529de5 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/feature_hydrator/IsListMemberFeatureHydrator.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/feature_hydrator/IsListMemberFeatureHydrator.scala
  
   package com.twitter.home_mixer.product.list_recommended_users.feature_hydrator
  
   
  
   import com.twitter.home_mixer.model.request.HasListId
  
  -import com.twitter.home_mixer.product.list_recommended_users.model.ListFeatures.IsListMemberFeature
  
  +import com.twitter.home_mixer.product.list_recommended_users.model.ListRecommendedUsersFeatures.IsListMemberFeature
  
   import com.twitter.product_mixer.component_library.model.candidate.UserCandidate
  
   import com.twitter.product_mixer.core.feature.Feature
  
   import com.twitter.product_mixer.core.feature.featuremap.FeatureMap
  
