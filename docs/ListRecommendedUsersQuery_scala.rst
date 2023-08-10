a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/model/ListRecommendedUsersQuery.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/model/ListRecommendedUsersQuery.scala
==================================================

Change Range: -18,7 +18,8

Content:

::

index 8d8c2e6ea..15b16c25b 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/model/ListRecommendedUsersQuery.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/list_recommended_users/model/ListRecommendedUsersQuery.scala
  
     override val debugOptions: Option[DebugOptions],
  
     override val features: Option[FeatureMap],
  
     selectedUserIds: Option[Seq[Long]],
  
  -  excludedUserIds: Option[Seq[Long]])
  
  +  excludedUserIds: Option[Seq[Long]],
  
  +  listName: Option[String])
  
       extends PipelineQuery
  
       with HasPipelineCursor[UrtUnorderedExcludeIdsCursor]
  
       with HasListId {
  
