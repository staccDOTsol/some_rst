a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/filter/PreviouslyServedUsersFilter.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/filter/PreviouslyServedUsersFilter.scala
==================================================

Change Range: -18,7 +18,7

Content:

::

index 97cd6a5c1..ac8c3107d 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/filter/PreviouslyServedUsersFilter.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/filter/PreviouslyServedUsersFilter.scala
  
   package com.twitter.home_mixer.product.list_recommended_users.filter
  
   
  
  -import com.twitter.home_mixer.functional_component.feature_hydrator.ListMembersFeature
  
  +import com.twitter.home_mixer.product.list_recommended_users.feature_hydrator.RecentListMembersFeature
  
   import com.twitter.home_mixer.product.list_recommended_users.model.ListRecommendedUsersQuery
  
   import com.twitter.product_mixer.component_library.model.candidate.UserCandidate
  
   import com.twitter.product_mixer.core.functional_component.filter.Filter
  
       candidates: Seq[CandidateWithFeatures[UserCandidate]]
  
     ): Stitch[FilterResult[UserCandidate]] = {
  
   
  
  -    val recentListMembers = query.features.map(_.getOrElse(ListMembersFeature, Seq.empty))
  
  +    val recentListMembers = query.features.map(_.getOrElse(RecentListMembersFeature, Seq.empty))
  
   
  
       val servedUserIds = query.pipelineCursor.map(_.excludedIds)
  
   
  
