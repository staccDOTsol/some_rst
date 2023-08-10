a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/model/FollowingQuery.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/model/FollowingQuery.scala
==================================================

Change Range: -47,4 +47,5

Content:

::

index 39442a61b..c45c7cf68 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/model/FollowingQuery.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/product/following/model/FollowingQuery.scala
  
     override val isEmptyState: Option[Boolean] = None
  
     override val isFirstRequestAfterSignup: Option[Boolean] = None
  
     override val isEndOfTimeline: Option[Boolean] = None
  
  +  override val timelineId: Option[Long] = None
  
   }
  
