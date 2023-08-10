a/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/request/HomeMixerProductContext.scala vs b/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/request/HomeMixerProductContext.scala
==================================================

Change Range: -30,5 +32,11

Content:

::

index 9f3ec4cb7..dddd733f3 100644
  
  --- a/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/request/HomeMixerProductContext.scala
  
  +++ b/home-mixer/server/src/main/scala/com/twitter/home_mixer/model/request/HomeMixerProductContext.scala
  
   case class ForYouProductContext(
  
     deviceContext: Option[DeviceContext],
  
     seenTweetIds: Option[Seq[Long]],
  
  -  dspClientContext: Option[DspClientContext])
  
  +  dspClientContext: Option[DspClientContext],
  
  +  pushToHomeTweetId: Option[Long])
  
       extends ProductContext
  
   
  
   case class ScoredTweetsProductContext(
  
     deviceContext: Option[DeviceContext],
  
     seenTweetIds: Option[Seq[Long]],
  
  -  servedTweetIds: Option[Seq[Long]])
  
  +  servedTweetIds: Option[Seq[Long]],
  
  +  backfillTweetIds: Option[Seq[Long]])
  
       extends ProductContext
  
   
  
   case class ListTweetsProductContext(
  
   case class ListRecommendedUsersProductContext(
  
     listId: Long,
  
     selectedUserIds: Option[Seq[Long]],
  
  -  excludedUserIds: Option[Seq[Long]])
  
  +  excludedUserIds: Option[Seq[Long]],
  
  +  listName: Option[String])
  
  +    extends ProductContext
  
  +
  
  +case class SubscribedProductContext(
  
  +  deviceContext: Option[DeviceContext],
  
  +  seenTweetIds: Option[Seq[Long]])
  
       extends ProductContext
  
